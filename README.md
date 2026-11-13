# VPS for Discord Bot: How Much RAM You Need, What JustHost Plans Cost Per Year, and How to Keep It Online (Full Plan Table)

A 1 vCPU / 1 GB VPS runs two or three text-based Discord bots without strain. An idle Node.js or Python bot holds a few dozen megabytes and spends most of its life waiting on the Discord gateway, so CPU is rarely the limit. What keeps it answering at 3 a.m. is a machine that never sleeps plus a process manager that restarts it after a crash.

On JustHost, the cheapest 1 GB plan is **New Star at $3.81/mo** (1 core, 10 GB NVMe, 100 Mbps port). **Sirius at $4.85/mo** doubles the disk to 20 GB and the port to 300 Mbps, and the same plan currently appears at **$2.81/mo** in the promo block on JustHost's VPS page. Under both sits a $1.99 plan with 512 MB of RAM, which is thin enough that it only makes sense for a single small bot.

> **Quick pick:** Sirius, 1 core / 1 GB RAM / 20 GB NVMe. Room for 2–3 text bots, logs, and a year of library updates. If 10 GB of disk is enough, New Star saves you $1.04 a month. 👉 [Check the current Sirius price on JustHost](https://justhost.asia/en/services/vps/tariffs/sirius?ref=196392)

## JustHost VPS plans, all of them

Prices below come from JustHost's full tariff list. Port speeds are unmetered but fair-share, so the number is a ceiling shared with other customers, not a guaranteed dedicated line.

| Plan | vCPU / RAM | NVMe | Port (fair-share) | Price | Order |
| --- | --- | --- | --- | --- | --- |
| VPS Promo | 1 core / 512 MB | 5 GB | 10 Mbps (~4+ TB/mo) | $1.99/mo | [Order](https://justhost.asia/en/services/vps/tariffs/promo-c1r512nvme?ref=196392) |
| New Star | 1 core / 1 GB | 10 GB | 100 Mbps (~31+ TB/mo) | $3.81/mo, $3.05 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/new-star-c1r1024nvme?ref=196392) |
| Sirius | 1 core / 1 GB | 20 GB | 300 Mbps (~93+ TB/mo) | $4.85/mo, $3.88 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/sirius?ref=196392) |
| Arcturus | 1 core / 2 GB | 30 GB | 300 Mbps | $6.92/mo, $5.53 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/arcturus?ref=196392) |
| Double Star | 2 cores / 2 GB | 20 GB | 200 Mbps | $7.08/mo, $5.67 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/double-star-c2r2048nvme?ref=196392) |
| Alpha Centauri | 2 cores / 2 GB | 40 GB | 400 Mbps | $8.99/mo, $7.19 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/alpha_centauri?ref=196392) |
| Bright Star | 4 cores / 4 GB | 30 GB | 300 Mbps | $9.80/mo, $7.84 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/bright-star-c4r4096nvme?ref=196392) |
| Vega | 2 cores / 4 GB | 80 GB | 500 Mbps | $13.44/mo, $10.75 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/vega?ref=196392) |
| Capella | 4 cores / 4 GB | 120 GB | 750 Mbps | $18.30/mo, $14.64 on 12-month | [Order](https://justhost.asia/en/services/vps/tariffs/capella?ref=196392) |

Above Capella the lineup continues with a 4-core / 8 GB / 200 GB tier (Rigel on the tariff list), then 6 cores / 12 GB / 400 GB and 8 cores / 24 GB / 600 GB. None of those make sense for a chat bot.

**One thing to know before you compare prices.** JustHost's two official pages disagree. The VPS landing page runs a "Special" promo block where Sirius is $2.81/mo, Arcturus $4.01, Vega $7.79, the 8 GB tier (listed there as Yildun) $16.20, Betelgeuse $26.40 and Altair $47.40, each marked down another 20% on 12-month billing. The full tariff list prices the same names higher. Both pages belong to JustHost, so treat the cart total as the real number and check it before paying.

## How much RAM does a Discord bot actually use?

Less than most people budget for. A typical Python or Node.js bot uses well under a few hundred megabytes, and an idle one waiting on the gateway sits at a few tens of megabytes. That is why 1 GB covers two or three lightweight bots plus the operating system, and why the 512 MB plan is only tight because Node plus a database driver plus the OS can approach the ceiling.

What actually moves you up a tier:

- **A database next to the bot.** PostgreSQL or Redis wants its own headroom. Move to 2 GB (Arcturus) and stop worrying.
- **Music bots.** Lavalink decoding audio is memory-hungry compared with text commands. 4 GB (Vega or Bright Star) is the sensible floor, and a 1 GB box will swap or get processes killed once a few people queue tracks.
- **Headless browsers or scrapers.** Count roughly 2 GB per concurrent browser. Three at once means 8 GB, which is well past anything a normal Discord bot needs.
- **A web dashboard or webhook endpoint** running on the same machine. Still small, but it is another process that has to stay up.

Sizing rules that hold up in practice:

| What you run | RAM | JustHost plan |
| --- | --- | --- |
| One small bot, minimum spend | 512 MB | VPS Promo, $1.99/mo |
| 2–3 text bots | 1 GB | New Star $3.81 or Sirius $4.85 |
| Bots + Postgres/Redis, or a dashboard | 2 GB | Arcturus $6.92 |
| Lavalink music bot | 4 GB | Vega $13.44 |
| Headless browser, per concurrent instance | ~2 GB each | Bright Star and up |

## Does a Discord bot need open ports or a domain?

No. Discord bots connect outward to the gateway and keep a websocket open, so nothing has to reach your server from the internet. No domain, no TLS certificate, no nginx. Bandwidth is trivial as well; even the 10 Mbps fair-share port on the $1.99 plan works out to several terabytes a month, which a text bot will never touch.

Telegram is the case that changes shape. Long polling needs nothing, but webhooks need a domain pointed at the server, HTTPS, and a reverse proxy such as nginx or Caddy in front. Payment callbacks usually push you toward webhooks too. If you plan to run both kinds of bot on one box, that is another argument for the 2 GB plan rather than the 1 GB one.

## What a year on JustHost actually costs

JustHost bills monthly or in prepaid terms. There is no hourly meter on these plans, so a two-day test costs a month rather than cents. The 12-month rate is where the real discount sits:

| Plan | Monthly billing, 12 months | 12-month billing, year one | You save |
| --- | --- | --- | --- |
| New Star | $45.72 | $36.60 | $9.12 |
| Sirius | $58.20 | $46.56 | $11.64 |
| Arcturus | $83.04 | $66.36 | $16.68 |
| Vega | $161.28 | $129.00 | $32.28 |

If the promo block applies at checkout, Sirius comes down to $27.00 for the first year on annual billing. That is the number to aim for, and it is also why you should look at the cart rather than the landing page.

Renewal is the part JustHost doesn't spell out. The 20% is tied to paying 12 months up front, and no separate renewal rate is published next to these prices. Plan for the annual rate in effect at each renewal, and if the bot is a hobby you might abandon in three months, pay monthly for a while first. The 1 GB tier costs about a dollar a month more that way, which is cheap insurance against a year-long commitment.

👉 [See JustHost's full VPS price list before you pick a term](https://justhost.asia/en/services/vps/tariffs-all?ref=196392)

## Keeping the bot online: systemd, pm2 or Docker

Starting a bot from an SSH session is fine for a test. It dies the moment you disconnect, and it stays dead after a reboot. Run it as a service instead. systemd is already on the server, and a unit file like this covers crashes and reboots:

ini
[Unit]
Description=discord bot
After=network-online.target

[Service]
User=bot
WorkingDirectory=/home/bot/app
EnvironmentFile=/home/bot/app/.env
ExecStart=/usr/bin/node index.js
Restart=always
RestartSec=5
MemoryMax=700M

[Install]
WantedBy=multi-user.target


Enable it once with `systemctl enable --now discordbot` and the bot comes back on its own. pm2 does the same job for Node projects and gives you a process list; Docker with `restart: unless-stopped` keeps dependencies tidy when several bots share a server.

The details that bite people later:

- **Run as a non-root user.** Create a `bot` account, own the app directory with it, nothing else.
- **Keep the token out of the code and out of Git.** Put it in a `.env` file with `chmod 600`, loaded through `EnvironmentFile`.
- **Cap the logs.** A chatty bot writing to journald can fill a 20 GB disk in weeks. Set `SystemMaxUse=200M` in `journald.conf`.
- **Cap the memory.** `MemoryMax=` turns a slow leak into a clean restart instead of an unresponsive machine.
- **Set the timezone before you write cron jobs.** Fresh servers run on UTC, and a "daily at 9am" reminder will fire at the wrong hour.
- **Add an uptime ping.** `Restart=always` can restart a broken bot all night without telling anyone.

## Limits to check before you pay

**Address family.** JustHost also sells IPv6-only configurations from $1.60/mo and IPv4+IPv6 configurations from $2.60/mo. A Discord bot only makes outbound connections, so IPv6-only can work, but anything that accepts inbound traffic (a webhook, a status page, an SSH habit that depends on a stable v4 address) needs the IPv4 option. Confirm what your chosen plan includes.

**Support depth.** Reviews on Trustpilot are mixed in a specific way. Long-term users report acceptable price for money on simple workloads like a VPN. The complaints are consistent about support: live chat that is only a bot, ticket replies taking up to 48 hours, an outage on 2 March 2026 with no status page, and one reviewer who lost a prepaid year to a "service violation" claim. For a hobby bot you can rebuild in ten minutes, that is a reasonable trade at $4.85 a month. For a bot that paying communities depend on, budget for the risk.

**Discord's rules.** Hosting a bot on a VPS is allowed and is how most public bots run. Discord's API cares about rate limits, not about where your process lives. Spam, scraping that violates a target site's terms, and abuse complaints can still get an account or a server suspended.

## How JustHost compares on monthly price

| Provider and plan | Specs | Price | Billing |
| --- | --- | --- | --- |
| JustHost New Star | 1 core / 1 GB / 10 GB NVMe | $3.81/mo | Monthly or prepaid terms |
| DataPasa VPS-1 | 1 vCPU / 1 GB / 16 GB NVMe / 1 TB | $1.81/mo ($0.0025/hr) | Hourly from prepaid balance |
| BuyVPS S-2 | 1 vCPU / 2 GB / 30 GB NVMe / unmetered | $7.99/mo, $7.19/mo annual, first month $1 for new customers | Monthly or annual |
| NoctHost Micro | 1 vCPU / 1 GB / 25 GB NVMe | $10/mo ($0.014/hr) | Hourly |
| OliveVPS Starter | 1 vCPU / 1 GB / 25 GB NVMe / 1 TB | $7.99/mo | Monthly, three fixed plans |

JustHost's 1 GB tier is the cheapest of the fixed-monthly options here, and it is the only one in this list without hourly billing. If your plan is to test a bot for a weekend and decide later, hourly billing elsewhere costs cents and JustHost costs a month. If you already know the bot is staying up, the monthly number is what matters, and $3.81 is hard to beat.

## Questions that come up

**Can one 1 GB VPS run several bots?** Yes. Two or three lightweight text bots, each as its own systemd service or container, fit comfortably. Bots that wait on websockets spend most of their time idle.

**Do I need a big CPU for a Discord bot?** No. Command handling is I/O-bound. A shared 1-core plan is fine until you add image processing, audio decoding, or tight polling loops.

**Can I run music bots on the cheap plan?** Technically yes, but Lavalink plus a queue of listeners pushes memory fast. Start at 4 GB, and expect to move up if several voice channels are active at once.

**What happens when the bot outgrows the box?** JustHost's VPS constructor lets you change specs on the same product line. Confirm with support whether an upgrade keeps your IP before you rely on it, since a new address means new DNS and a re-login for anything that pins it.

## Picking your plan

One small bot and a tight budget: the $1.99 VPS Promo, accepting that 512 MB leaves almost no room. Two or three text bots: Sirius at $4.85, or $2.81 if the promo holds. Bots plus a database, a dashboard, or a Telegram webhook: Arcturus at $6.92. Music bot or a browser: Vega at $13.44, and add roughly 2 GB per extra concurrent browser.

Before you pay, check three things in the cart: whether the promo price or the tariff-list price is applied, whether the plan includes IPv4, and whether you are committing to one month or twelve. Then write the systemd unit before you invite the bot to a server, so the first crash isn't the one that keeps it offline until morning.

👉 [Compare all JustHost VPS plans and current prices](https://justhost.asia/en/services/vps/tariffs-all?ref=196392)
