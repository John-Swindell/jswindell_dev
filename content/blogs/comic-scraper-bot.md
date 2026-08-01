---
title: "The Birthday Bot"
date: 2026-01-31
image: "/images/projects/discord_bot_image_project.webp"
description: "A small async Python bot I built to help a friend catch limited comic book releases."
summary: "A small async Python bot I built to help a friend catch limited comic book releases."
source: "https://github.com/John-Swindell/comic-scraper-discord-bot"
tags: ["Python", "Asyncio", "Docker", "Developer Experience", "Discord API"]
---
{{< button link="https://jswindell.dev" >}}
Back to Home Page
{{< /button >}}

{{< button link="https://github.com/John-Swindell/comic-scraper-discord-bot" >}}
View Source Code
{{< /button >}}

{{< button link="https://jswindell.dev/blogs" >}}
View More Blogs
{{< /button >}}

<br>
<br>
<br>

### Why I made it

This started as a birthday gift for a close friend. He is a UX engineer and a serious comic collector, but he kept missing limited variant covers that would appear without much warning and sell out in minutes.

I wanted to give him an alert before the books disappeared. I also wanted the project to be readable enough that he could change the search rules himself without having to learn an entire backend stack first.

### Keeping the bot responsive

The bot has two jobs. It checks a store for new listings and keeps a live Discord connection open so it can send an alert. A normal synchronous request can pause everything while it waits for the store, which is a good way to make the Discord connection unreliable.

I used Python with asyncio and aiohttp so the scraper can wait on a request without blocking the rest of the bot. The core loop is deliberately small.

    async def monitor(client):
        async with aiohttp.ClientSession(headers=HEADERS) as session:
            while not client.is_closed():
                try:
                    found_items = await check_page(session)
                    await send_alerts(client, found_items)
                except Exception as error:
                    print(f"Network issue, retrying: {error}")

                await asyncio.sleep(60)

It also reconnects after network interruptions and can test its parsing against saved HTML. That offline test path matters because it lets someone change keywords or selectors without repeatedly hitting the live site.

### Making it easy to change

Because I was handing the code to a friend, I wrote the comments as short explanations of the unfamiliar parts. They cover why await is used, what the event loop is doing, and where to change the matching rules. The goal was not just to give him a running bot. It was to leave him with code he could own.

We put it in a Docker container and configured it to restart with the server. A few weeks later it found the exact kind of release he had been missing, and he bought the only available copy. A couple of friends have since adapted the repository to watch for other hobbies, which is about the best outcome I could have wanted from a small birthday project.
