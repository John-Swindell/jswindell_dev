---
title: "The Birthday Bot: Async Scraping for a UX Engineer"
date: 2026-01-31
image: "/images/projects/discord_bot_image_project.webp"
description: "How I built a non-blocking Python scraper as a gift for a friend, containerized it for 24/7 uptime, and helped him snag a rare variant."
summary: "How I built a non-blocking Python scraper as a gift for a friend, containerized it for 24/7 uptime, and helped him snag a rare variant."
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

### The Backstory
This project wasn't built for a client, it was a birthday gift for a close friend.

He is a Senior UX Engineer at YouTube, brilliant with frontend interactions and user flows, but he admits he isn't as familiar with backend infrastructure or Python scripts. He is also a massive comic book collector who kept missing out on limited "virgin" variant covers because they would appear unlisted and sell out in minutes. 

Virgin, for those who don't know comic book speak, just means the logos, barcode, and cover title are removed from the front. These variant covers are created solely to focus on the artwork.

I wanted to give him a tool that would give him an edge, but I also wanted it to be a learning resource. It needed to be robust enough to run 24/7, but clean enough that he could read the code, understand the logic, and eventually tweak it himself.

### The Objective
Build a highly responsive monitoring tool that detects unlisted product variants the second they go live. The technical challenge was concurrency: maintaining a persistent WebSocket connection to Discord for alerts while simultaneously polling a web server, without one task blocking the other.

### The Solution: Non-Blocking Architecture
I architected a non-blocking asynchronous loop using Python's `asyncio` and `aiohttp`. Unlike standard synchronous requests (which pause the entire program), this architecture allows the bot to "heartbeat" with the Discord API while waiting for network responses from the target site.

Because I was handing this code off, I treated Developer Experience (DX) as a first-class feature. I filled the codebase with "Dev Specific Notes" like mini-tutorials inside the comments explaining *why* we use `await` or how the event loop works, bridging the gap between his UX background and backend execution.

### Key Engineering Features

1.  **Non-Blocking I/O:** Utilized `aiohttp.ClientSession` to perform HTTP GET requests without halting the Event Loop. This prevents the "Connection Timeout" crashes common in simpler bots.
2.  **Resilient Error Handling:** Implemented auto-recovery logic for WebSocket disconnects and network stutters. This was crucial because the bot needed to run unsupervised on a VPS/Raspberry Pi.
3.  **Offline Testing Suite:** I built a custom sandbox environment (`test_scraper.py`) that runs parsing logic against local HTML snapshots. This allowed my friend to test new search keywords safely without worrying about triggering the site's anti-bot IP bans.

### The Code: The Event Loop

*Snippet below demonstrates how the bot balances the WebSocket heartbeat with the scrape job.*
```python
# The bot maintains the WebSocket heartbeat while awaiting HTTP responses
async def monitor(client):
    async with aiohttp.ClientSession(headers=HEADERS) as session:
        while not client.is_closed():
            try:
                found_jobs = await check_page(session) # Non-blocking call
                # ... alert logic ...
            except Exception as e:
                print(f"Network glitch, retrying: {e}")
            
            # Yields control back to the loop, keeping the bot "alive"
            await asyncio.sleep(60) 

```

### The Win: From Code to Container

The final step was deployment. We sat down and I walked him through **containerizing the application with Docker**. We set it up to run as a background service, ensuring it would restart automatically if the server rebooted.

Many weeks later, it paid off. The bot pinged him mid afternoon about the drop. Because he got the push notification instantly, he was able to secure the only copy available before it vanished.

Since then, the repo has taken on a life of its own. A couple of our other mutual friends have cloned it to track their own hobbies. It started as a script, but it became a very small community tool.
