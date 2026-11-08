# Palworld Dedicated Server Hosting: How Many Players Fit, How Much RAM You Need, and When to Skip the Game Panel for a $115 Bare-Metal Box (Full Plan Comparison)

Spinservers' Palworld-ready VPS is listed at **$25/mo** with 4 cores and 16 GB of RAM in San Jose. Its instant dedicated servers include a **$115/mo** configuration with 64 GB of RAM. The VPS covers a private world of roughly 8 to 20 players. The dedicated machine is for communities that sit at the 32-player cap with mods loaded and don't want to watch RAM graphs every night.

Below: what each option includes, why Palworld's memory use grows even when nobody new logs in, what crossplay actually covers right now, the costs that don't appear in the monthly price, and how a self-hosted Docker server compares with renting one.

> **Most readers should take the $25/mo Palworld-ready VPS.** It has twice the RAM that one host's FAQ calls the minimum for a Palworld server, fits in a single San Jose location, and you get root access to install mods yourself. Pick the **$115/mo SJC-202 dedicated server** instead if you run 20+ players with heavy mods, several worlds on one machine, or need more than 16 GB of headroom.
>
> 👉 [Check the Palworld-ready VPS and current San Jose availability](https://bit.ly/Spinservers)

## What Spinservers lists for Palworld

Spinservers doesn't sell Palworld the way game-panel hosts do, with a slider for player slots and a “buy” button per world. It sells a pre-sized VPS aimed at Palworld, and above that instant dedicated hardware in San Jose that you provision and configure yourself. That changes the buying decision: you're choosing RAM, cores and a location, then installing the server software.

| Option | CPU / RAM | Storage / network | Price | Best for | Order |
| --- | --- | --- | --- | --- | --- |
| Palworld-ready VPS (San Jose) | 4 cores, 16 GB RAM | 40 GB SSD, 10 TB traffic on a 1 Gbps port | **$25/mo** | 8–20 player private worlds, vanilla or lightly modded | [View this VPS](https://bit.ly/Spinservers) |
| SJC-202 instant dedicated | Dual Intel Xeon E5-2630L v3, 64 GB RAM | 1.6 TB SSD, 10 TB traffic on a 10 Gbps port | **$115/mo** | 30+ player communities, mod stacks, multiple instances | [View this dedicated server](https://bit.ly/Spinservers) |
| Larger instant dedicated | Up to 32 cores / 64 threads, up to 512 GB RAM | Up to 2×3.84 TB SSD on hardware RAID, 2×10 Gbps ports | Shown on the order form | Several worlds per box, mod testing, long-term communities | [See the full dedicated lineup](https://bit.ly/Spinservers) |

Other instant San Jose configurations on the same order form, if you need more than the two above:

| CPU | RAM | Storage | Network |
| --- | --- | --- | --- |
| 4 cores / 8 threads, 3.7–4.0 GHz | 32 GB DDR4 ECC | 1 TB M.2 NVMe | 2×10 Gbps, 30 TB transfer |
| 16 cores / 32 threads, 1.8–2.9 GHz | 64 GB DDR4 ECC | 1.6 TB NVMe | 2×10 Gbps, 30 TB transfer |
| 16 cores / 32 threads, 1.8–2.9 GHz | 256 GB DDR4 ECC | 2×1.6 TB SSD with RAID cache | 2×10 Gbps, 30 TB transfer |
| 32 cores / 64 threads, 2.1–3.0 GHz | 512 GB DDR4 ECC | 2×3.84 TB SSD with battery-backed RAID | 2×10 Gbps, 30 TB transfer |

Provisioning on these is instant, and the OS is your call (Ubuntu 24.04 LTS appears as a template). One thing worth noting before you fall in love with the 10 Gbps port: Palworld traffic is tiny. A full 32-player server moves a few Mbps. Bandwidth isn't what you're buying here.

## How many players can a Palworld server actually hold?

**32.** The dedicated server software takes a max-player value between 1 and 32, and the Docker image most self-hosters use defaults to 16. That ceiling doesn't move because a host advertises something bigger.

That number causes confusion because hosts describe capacity in different units. Vastrox's Palworld page says 32 players in the heading, then lists 3, 8 and 15 slots on its three plans. Gravel Host and NovaCloud both advertise unlimited player slots. Those figures describe billing, not the game: a “slot” is what the host meters or doesn't meter, while the server's own config file still tops out at 32. Spinservers skips the slots language entirely and sells you a machine, so your player count is whatever you write into the config, limited by whether the box keeps up.

For a 16 GB VPS, 8 to 20 players in a developed world is a realistic range. Beyond that, base size and Pal count matter more than the number of logins.

## How much RAM does a Palworld server need?

**8 GB is the floor for a small world; 16 GB is comfortable; 64 GB is for communities that refuse to prune.** NovaCloud's FAQ recommends 8 GB minimum and 16 GB or more for larger servers.

The trap is assuming RAM scales with concurrent players. It scales with the world. Every base Pal, captured Pal and dropped item stays resident in memory, and usage climbs the longer the process runs. That's why scheduled restarts are standard practice on Palworld servers, as Vastrox's documentation puts it: restarts keep memory in check without anyone babysitting uptime.

Two practical consequences:

- A 4 GB plan is a trial, not a home. Gravel Host's $9/mo Chikipi tier and Vastrox's $17.49/mo Co-op plan both sit at 4 GB. Fine for two weeks of testing with three friends; tight once you've got three bases and a breeding operation.
- Restart scheduling is part of your purchase decision. On a managed panel it's a setting. On a Spinservers VPS or dedicated box, you're writing the cron job or Docker restart policy yourself.

If your group is at 25–32 players with big bases and mods that add spawns, 16 GB will feel tight within a month. That's the point where the 64 GB dedicated configuration stops being overspend.

👉 [Compare the 16 GB VPS against the 64 GB dedicated server](https://bit.ly/Spinservers)

## Crossplay: what's confirmed and what isn't

Steam and the Xbox / Microsoft Store PC version can share a world. NovaCloud documents that pairing explicitly, with one server and one shared save.

The rest is murkier. The Docker project's example compose file includes a crossplay setting listing `(Steam,Xbox,PS5,Mac)`, but that's a container configuration example, not an official support list, and it doesn't prove PS5 or Mac clients can join your server today. If your group includes console players, ask support to confirm the current platform list for the build you'd be running before you pay. Buying on the assumption that PS5 players will join is how refund requests happen.

There's a related point that applies to every host: after a Pocketpair patch, the server has to run the same build as your players' clients, or nobody can connect. On a managed host that's a one-click update. On a Spinservers box, it's your job, and it's the single biggest reason people move from self-managed to managed hosting.

## Mods: what you get and what you install

The Docker image ships with mod support enabled by default. You pass Steam Workshop IDs as a comma-separated list in `MOD_IDS`, and it handles UE4SS and PalSchema out of the box, with PalDefender as a toggle. Mod updates on boot are on by default too.

On a Spinservers VPS or dedicated server, root access means all of this is available, and it also means all of it is your responsibility. On a game panel host with “mod support,” you're usually ticking a box and uploading files through a file manager.

The cost of mods isn't disk space. It's RAM. Mods that raise spawn density or add Pals push the memory creep faster, which is exactly the scenario where the 16 GB VPS stops being enough and the 64 GB machine earns its price.

## The real total cost (and the refund question)

Monthly price is the least interesting number on the page.

The order form offers annual billing on San Jose configurations; one listing shows **$660/yr, or $55/mo effective**, against a $115 monthly rate. That's roughly 52% off if you keep the server a full year, and roughly a 52% loss if you cancel in month two and the refund policy doesn't cover annual terms.

Refund terms vary wildly across this market, which tells you not to assume anything:

| Host | Entry price | Refund terms as published |
| --- | --- | --- |
| Spinservers | $25/mo VPS, $115/mo dedicated | Confirm current terms before paying annually |
| Vastrox | $17.49/mo (4 GB) | 30-day money-back |
| Gravel Host | $9.00/mo (4 GB) | 72 hours, first purchase only, once per account, minus processing fees; excludes renewals, add-ons and upgrades; annual plans may be prorated within 7 days |
| NovaCloud | €3/mo (2 GB) | Not stated on the product page |
| iDatam | $79/mo bare metal (16 GB+) | Not stated on the product page |

Note the currencies and product types: NovaCloud prices in euros, iDatam sells bare metal from $79/mo with 16–128 GB RAM, Vastrox and Gravel Host sell game-panel plans at 4 GB. Sorting these by headline price tells you nothing useful.

Two other cost items that don't show up monthly:

**Location.** Spinservers' Palworld VPS and instant servers are in San Jose. If most of your players are in Europe or Southeast Asia, test ping before committing to a year. A cheap server nobody can play on is the most expensive option available.

**DDoS protection.** NovaCloud states plainly that it can't guarantee complete mitigation of every attack, and Gravel Host says protection doesn't guarantee uninterrupted access. Nobody credible promises 100%. If DDoS filtering is a requirement for you, get the answer in writing before you order rather than assuming it's bundled.

👉 [Check current Spinservers pricing and billing terms before you prepay](https://bit.ly/Spinservers)

## Self-hosted Docker vs. renting a server

Running Palworld yourself in Docker is genuinely cheap, and the defaults are better than most people expect.

**What's on by default:** automatic backups, once a day at midnight. The REST API on TCP 8212. Mod support. Update-on-boot for the container itself.

**What's off by default:** automatic updates of the game server (`AUTO_UPDATE_ENABLED` is false, with an hourly cron ready if you turn it on), RCON, auto-reboot, and the performance threading flags (`-useperfthreads -NoAsyncLoadingThread -UseMultithreadForDS`), which you enable with one variable and can pair with a worker-thread count.

**Ports to know:** 8211/UDP for the game, 27015/UDP for the community server browser, 8212/TCP for the REST API. The project explicitly warns not to forward the REST API port. It's enabled by default and it's an administrative interface, so exposing it to the internet is a mistake people make once.

What you're really trading when you self-host:

|  | Self-hosted (VPS or dedicated) | Managed game panel |
| --- | --- | --- |
| Monthly cost | Lower for the same RAM | Higher per GB of RAM |
| Patch day | You update, or players can't join | One-click, host handles timing |
| Backups | Daily by default, restore is your job | Usually included with a restore button |
| Ports and security | You manage the firewall | Handled |
| Mods | Full control, full responsibility | Usually a checkbox |

Self-hosting suits you if you've run a Linux server before, you're comfortable with Docker Compose and cron, and you'll actually apply patches the day Pocketpair ships them. Renting a panel suits you if “I'll update it this weekend” describes your usual timeline, because in Palworld that timeline means a dead server.

Spinservers sits on the self-hosted side of that line even though you're renting. You get the hardware and root access; the operational work stays yours.

## Before you order: seven things to confirm

1. **Refund window and whether it covers annual billing.** Get it in writing if you're paying for a year.
2. **Renewal price.** The $660/yr figure is an annual rate, not a promise about year two.
3. **Crossplay platform list** for the build you'll run, if any of your players are on console.
4. **Latency from your players' region to San Jose.**
5. **DDoS filtering:** included, and to what level.
6. **Backup retention and restore process** on the VPS image you pick.
7. **Who applies patches**, and how fast after a Pocketpair release.

## FAQ

**Can I run mods on the $25 Palworld VPS?**
Yes. You have root access, and the standard Docker image supports Steam Workshop mods through `MOD_IDS` with UE4SS and PalSchema bundled. Budget extra RAM for mods that add Pals or raise spawn density.

**How many players will 16 GB support?**
The server caps at 32 regardless. In practice, 16 GB handles roughly 8 to 20 players in a developed world with scheduled restarts. Above that, base and Pal count decide it.

**Do Palworld saves survive patches?**
Yes. Bases and Pals persist across updates. The server must run the same build as clients, so apply patches promptly.

**Is a $9/mo 4 GB game plan enough instead?**
For two weeks of testing with three friends, it's fine. For a world you intend to keep, 4 GB is the number people upgrade away from first.

## Which one to pick

- **2 to 10 friends, vanilla or lightly modded:** the $25/mo Palworld-ready VPS. 16 GB is double the recommended minimum and leaves room for a mod or two.
- **15 to 32 players, big bases, mod stacks:** the $115/mo SJC-202 with 64 GB. Restart weekly, not nightly.
- **Several worlds or a mod-testing server alongside the live one:** a 256 GB or 512 GB instant configuration, so you're not running production and experiments on the same RAM.
- **Mostly EU or Asia players:** check ping to San Jose before anything else. Location beats specs.

One last thing worth doing before you pay: open the order form and read the billing and refund terms for the exact configuration you selected. The gap between a $25 monthly VPS and a $660 annual commitment is large enough that the fine print deserves five minutes.

👉 [Start with the Palworld-ready VPS or size up to a dedicated box](https://bit.ly/Spinservers)
