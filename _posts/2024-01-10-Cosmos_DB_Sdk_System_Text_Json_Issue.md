---
layout: post
title: "System.Text.Json Serialization Issue With Azure Cosmos DB SDK V3 For dotnet8"
date: 2024-01-10 07:40:00 -0100
categories: coding dotnet azure
tags: c# dotnet cosmos_db azure system.txt.json
image:
  path: /assets/img/headers/cosmosdb.webp
---

### TL;DR

The Azure Cosmos DB SDK for .NET uses Newtonsoft.Json as the default serializer. However, we prefer System.Text.Json because it is more lightweight and performs better,
and System.Text.Json is the default serializer for .NET 6 and later. To solve the Serialization issue please copy and paste the code and everything should work.

---

Nowadays everyone wants to use System.Text.Json as the Default Serializer for their dotnet project but there are still some issues with this.

Recently while working on a project I was using Azure Cosmos Db with dotnet 8 and I decided to use Cosmos DB SDK v3, I managed to create the code and when I started debugging
I faced an issue, the error was <strong>required field id is missing</strong>, but in my model class I had an Id property and I added the attribute JsonPropertyName from System.Text.Json

When I checked the dependency of the Azure Cosmos Db SDK v3, I found that it is using NewtonSoft.Json, So when internally it was calling cosmos db it was unable to parse the ID.

So after digging into the internet I found some open Github issues and found answers, so I am writing this so you can save some time.

Create a `CosmosSystemTextJsonSerializer` class, and paste the following code.

```cs
public class CosmosSystemTextJsonSerializer : CosmosSerializer
{
    private readonly JsonObjectSerializer systemTextJsonSerializer;

    public CosmosSystemTextJsonSerializer(JsonSerializerOptions jsonSerializerOptions)
    {
        this.systemTextJsonSerializer = new JsonObjectSerializer(jsonSerializerOptions);
    }

    public override T FromStream<T>(Stream stream)
    {
        using (stream)
        {
          if (stream.CanSeek && stream.Length == 0)
          {
              return default;
          }

          if (typeof(Stream).IsAssignableFrom(typeof(T)))
          {
             return (T)(object)stream;
          }

          return (T)this.systemTextJsonSerializer.Deserialize(stream, typeof(T), default);
        }
    }

    public override Stream ToStream<T>(T input)
    {
        MemoryStream streamPayload = new MemoryStream();
        this.systemTextJsonSerializer.Serialize(streamPayload, input, input.GetType(), default);
        streamPayload.Position = 0;
        return streamPayload;
    }
}
```

And then while creating the client, pass the options with a custom serializer.

```cs
JsonSerializerOptions jsonSerializerOptions = new JsonSerializerOptions()
{
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
};
CosmosSystemTextJsonSerializer cosmosSystemTextJsonSerializer = new CosmosSystemTextJsonSerializer(jsonSerializerOptions);
CosmosClientOptions cosmosClientOptions = new CosmosClientOptions()
{
    ApplicationName = "Test",
    Serializer = cosmosSystemTextJsonSerializer
};
var client = new CosmosClient(endpoint, authKey, cosmosClientOptions);
```

I hope this article will help you.
cheers :)
