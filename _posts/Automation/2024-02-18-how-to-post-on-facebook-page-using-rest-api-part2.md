---
layout: post
title: "Maximizing Your Facebook Page: Part 2 - Creating Post using REST Apis"
date: 2024-02-18 05:38:00 00
categories: ferret
tags: facebook rest post api
image:
  path: /assets/img/headers/automation/unlock-your-facebook-posting-potential-2.webp
---

Hey there, welcome back to our journey in supercharging your Facebook Page! In our [last chat](/posts/how-to-post-on-facebook-page-using-rest-api-part1), we talked about getting your hands on that special API access token tailor-made for creating posts on your Page. Now, armed with that token, let's dive into the fun part – crafting and scheduling posts using REST APIs.

## Why REST APIs, You Ask?

Alright, so let's break it down. REST APIs are like the magic wand for tech folks. They let you talk to all sorts of web services, and in our case, Facebook's API is the one we're interested in. By tapping into these APIs, you can weave Facebook right into your existing tools and workflows. It's like having a social media genie at your command, ready to whip up posts whenever you need them!

## Getting Cozy with the Facebook Graph API

Now, before we jump into the action, let's chat about the Facebook Graph API. Think of it as your backstage pass to Facebook's world. This API is your gateway to interacting with Facebook's data and features. And guess what? There's a whole set of tools just for managing your Page's posts!

## Creating a Post

- Make sure you have the page token generated on [part 1](https://blogs.bitesinbyte.com/posts/how-to-post-on-facebook-page-using-rest-api-part1/#generate-an-api-access-token)
- Url: https://graph.facebook.com/v19.0/PAGE_ID/feed

  - Method: POST
  - Payload:

    - **message** set to the text for your post
    - **link** set to your URL if you want to post a link
    - **published** set to true to publish the post immediately (default) or false to publish later
      - Include scheduled_publish_time if set to false with the date in one of the following formats:
        - An integer UNIX timestamp [in seconds] (e.g. 1530432000)
        - An ISO 8061 timestamp string (e.g. 2018-09-01T10:15:30+01:00)
        - Any string otherwise parsable by PHP's strtotime() (e.g. +2 weeks, tomorrow)
    - **Notes about scheduled posts**

      - The publish date must be between 10 minutes and 30 days from the time of the API request.
      - If you are relying on strtotime()'s relative date strings you can read-after-write the scheduled_publish_time of the created post to make sure it is what is expected.

    - **Example Request**

      ```json
      {
        "message": "Text",
        "link": "https://blogs.bitesinbyte.com//posts/odata/",
        "access_token": "TOKEN",
        "published": "false",
        "scheduled_publish_time": "unix_time_stamp_of_a_future_date"
      }
      ```

    - **Response**
      Example of a successful response
      ```json
      {
        "id": "page_post_id"
      }
      ```
    - For more options please have a look into meta´s [documentation](https://developers.facebook.com/docs/graph-api/reference/v19.0/page/feed)

## Audience targeting

To limit who can see a Page post, you can add the targeting.geo_locations object or feed_targeting.geo_locations parameter in your POST request.

```json
{
  "targeting": {
    "geo_locations": {
      "countries": ["IN"],
      "cities": [
        {
          "key": "123",
          "name": "Bangalore"
        }
      ]
    }
  }
}
```

## Publish Media Posts

- [Publish a photo](https://developers.facebook.com/docs/graph-api/reference/page/photos/)

- [Publish a video](https://developers.facebook.com/docs/video-api/guides/publishing)

## Best Practices for Post Perfection

Hey, crafting posts is an art form, right? Here are a few tips to make sure your creations shine:

- **Keep It Fresh**: Nobody likes stale content. Keep your posts engaging and relevant to keep your audience coming back for more.
- **Timing Is Key**: Don't bombard your followers with posts all at once. Spread them out strategically to keep your audience engaged without overwhelming them.
- **Listen and Learn**: Pay attention to how your audience interacts with your posts. Use those insights to fine-tune your posting strategy and keep the engagement train rolling.
- **Stay on the Nice List**: Remember to play by Facebook's rules. Stick to their guidelines and policies to keep your Page in good standing.

## Wrapping Up

And there you have it – a crash course in creating Facebook posts like a pro using REST APIs! With these tools in your arsenal, you're ready to take your Page to new heights.

## Other

- [https://developers.facebook.com/docs/pages-api/posts/](https://developers.facebook.com/docs/pages-api/posts/)
