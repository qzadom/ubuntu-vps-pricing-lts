# vps ubuntu: Pick the Right LTS, Size the Plan, and Know When a China-Optimized Box Is Worth $39+

Two decisions hide inside that search: which Ubuntu release to install, and what hardware to put it on. The first has a boring answer, Ubuntu 24.04 LTS. The second depends entirely on where your users sit. If they're scattered globally, a $5–8/month KVM box is plenty. If a meaningful share of them are in mainland China, the thing you're paying for stops being CPU cores and becomes the network route.

This walks through both, then spends most of its space on GoMami Networks, a provider whose Ubuntu templates and pricing need a closer read than the homepage gives you.

> **Quick picks**
> - **Default:** Ubuntu 24.04 LTS on the smallest plan that fits your RAM. GoMami's cheapest published plan is the EPYC 7763 Mini (2 vCPU, 4GB RAM, 40GB NVMe, 1TB traffic, 1Gbps port) at **$39/month**.
> - **Single-core matters** (game server, realtime app, compile box): the Ryzen 9 9950X Mini, same 2 vCPU / 4GB / 40GB / 1TB shape, at **$59/month**.
> - Both deploy with a 24.04 LTS template selected at checkout.
> 👉 [See all six GoMami plans and current prices](https://bit.ly/Gomami)

## Which Ubuntu release belongs on a VPS

Ubuntu 24.04 LTS, unless you have a specific reason otherwise. Among the versions hosting providers actually hand you by default, it offers the longest remaining standard support and the least friction: Docker images, PPAs and nearly every tutorial you'll find at 2am assume it or 22.04.

Canonical's release list, current as of mid-October 2026:

| Release | Released | Standard support ends | What it's for |
| --- | --- | --- | --- |
| 26.10 | Oct 15, 2026 | mid-2027 | Interim, nine months of life. Not for anything you care about |
| 26.04 LTS | Apr 23, 2026 | 2031 | Newest LTS; verify your provider ships it |
| 24.04 LTS | Apr 25, 2024 | 2029 | The default answer |
| 22.04 LTS | Apr 21, 2022 | 2027 | Software only tested against it |
| 20.04 LTS | Apr 23, 2020 | ended 2025 | Migrate. ESM runs to 2030 if you need room |
| 18.04 LTS | Apr 26, 2018 | ended 2023 | Legacy only. ESM to 2028 |

Two point releases landed recently and matter more than they sound: 24.04.5 on September 10, 2026 and 26.04.1 on August 27, 2026. A fresh install today gets a mature image rather than a .0 that ages badly on first boot.

The "standard support ends" column is the one that counts. Everything after it, the extra five years of Expanded Security Maintenance and then the paid Legacy add-on that pushes coverage out further, needs an Ubuntu Pro subscription attached to the machine. Free tiers exist, but it's one more thing to wire up, and it isn't automatic.

The interim releases are the trap. 26.10 shipped on October 15, 2026 and stops receiving security patches in mid-2027. On a laptop that's fine. On a box serving traffic, you've just scheduled a migration for a year from now in exchange for a newer kernel.

## Where GoMami fits, and where its Ubuntu support stops

GoMami's official OS templates list Ubuntu 22.04 LTS and 24.04 LTS, plus Debian 11/12, CentOS Stream 9, AlmaLinux 8/9 and Windows Server 2019/2022. There is no 26.04 LTS template, nearly six months after Canonical shipped it.

That's not a blocker, but it changes the workflow. Anything outside the template list goes through custom ISO mounting: pick an ISO, set the boot order to CD-ROM, reboot manually because the panel won't do it for you, then finish the install through the VNC console. Afterwards, unmount the ISO and set the boot order back to Hard Disk. Skip that last part and the server boots into the installer again on the next restart, which is an annoying way to lose twenty minutes.

For the 24.04 crowd, none of this applies. Pick the template, wait a few minutes, get an IP and root password by email. Docker, Nginx and the usual stack install the way they do anywhere else; the provider publishes walkthroughs for the Docker repository setup, `ufw` rules for Nginx, and `unattended-upgrades` for automatic patching.

The reason anyone pays GoMami's prices at all is the network. The company positions itself around China Telecom CN2, Unicom 9929 and Mobile CMIN2 routing, with a homepage claim of under 50ms RTT across mainland China and up to 600 Gbps of DDoS mitigation. Both are vendor claims; no independent benchmark backs them in the material available. The triple-route approach, though, is a concrete thing generic $6 hosts don't do, and it affects you in unglamorous ways. `apt` mirrors, Docker Hub pulls and `npm install` all travel that route.

👉 [Check GoMami's current plan list and China-optimized routing](https://bit.ly/Gomami)

## Every GoMami plan, and why the pricing is split across two places

Read this before the tables: GoMami publishes prices and specs on different pages, and the same tier name carries different specs depending on which page you're on. The homepage lists six plans with prices but does not state the node or the product series. The store pages list series and full specifications but show no price until checkout. HKG.Turin.Mini, for instance, comes with 100GB of disk on the store page, while the homepage Mini on the same CPU platform shows 40GB.

So confirm the exact configuration and the price on the order page before paying. The tier name alone won't tell you which one you're buying.

**What the homepage publishes**, which is the only place official prices appear:

| Plan | CPU platform | vCPU / RAM / NVMe | Traffic / port | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- | --- |
| Mini | Ryzen 9 9950X (up to 5.7GHz) | 2 / 4GB / 40GB | 1000GB / 2Gbps | $59 | [ Order](https://bit.ly/Gomami) |
| Air | Ryzen 9 9950X | 4 / 8GB / 60GB | 2000GB / 2Gbps | $99 | [ Order](https://bit.ly/Gomami) |
| Pro | Ryzen 9 9950X | 6 / 16GB / 80GB | 5000GB / 5Gbps | $199 | [ Order](https://bit.ly/Gomami) |
| Mini | EPYC 7763 (up to 3.5GHz) | 2 / 4GB / 40GB | 1000GB / 1Gbps | $39 | [ Order](https://bit.ly/Gomami) |
| Air | EPYC 7763 | 4 / 8GB / 60GB | 2000GB / 1Gbps | $79 | [ Order](https://bit.ly/Gomami) |
| Pro | EPYC 7763 | 8 / 16GB / 80GB | 5000GB / 3Gbps | $169 | [ Order](https://bit.ly/Gomami) |

**What the store pages publish**, organised by series instead of by CPU platform. No prices are shown on these pages:

| Series | Plan | vCPU / RAM | NVMe | Traffic / port | Plans page |
| --- | --- | --- | --- | --- | --- |
| HKG.Turin | Mini | 2 / 4GB | 100GB | 1000GB / 2Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Turin | Air | 4 / 8GB | 140GB | 2000GB / 2Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Turin | Pro | 6 / 16GB | 180GB | 5000GB / 5Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Turin | Ultra | 12 / 32GB | 220GB | 10000GB / 5Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Pulse | Nano | 2 / 2GB | 40GB | 500GB / 1Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Pulse | Mini | 2 / 4GB | 60GB | 1000GB / 1Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Pulse | Air | 4 / 8GB | 80GB | 2000GB / 1Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Pulse | Pro | 8 / 16GB | 100GB | 5000GB / 3Gbps | [ View](https://bit.ly/Gomami) |
| HKG.Pulse | Ultra | 12 / 32GB | 300GB | 10000GB / 3Gbps | [ View](https://bit.ly/Gomami) |
| LAX.Pulse | Nano | 2 / 2GB | 40GB | 1000GB / 1Gbps | [ View](https://bit.ly/Gomami) |
| LAX.Pulse | Mini | 2 / 4GB | 60GB | 2000GB / 1Gbps | [ View](https://bit.ly/Gomami) |
| LAX.Pulse | Air | 4 / 8GB | 80GB | 4000GB / 2Gbps | [ View](https://bit.ly/Gomami) |
| LAX.Pulse | Pro | 6 / 16GB | 100GB | 8000GB / 3Gbps | [ View](https://bit.ly/Gomami) |
| LAX.Pulse | Ultra | 12 / 32GB | 300GB | 15000GB / 5Gbps | [ View](https://bit.ly/Gomami) |
| LAX.Pulse | Titan | 12 / 32GB | 600GB | 30000GB / 10Gbps | [ View](https://bit.ly/Gomami) |

**Three more series exist in the store sidebar** that neither table above covers. Their tier ladders are confirmed; their prices are not published on the store pages and only appear at checkout:

| Series | Location | Tiers listed on the store page | Note |
| --- | --- | --- | --- |
| HKG.Forge | Hong Kong | Mini, Air | Dedicated-server line rather than a shared VPS |
| JPN.Pulse | Tokyo | Nano, Mini, Air, Pro, Ultra | Same Pulse ladder as Hong Kong |
| SIN.Pulse | Singapore | Nano, Mini, Air, Pro, Ultra | Same Pulse ladder as Hong Kong |

In total: the homepage advertises Hong Kong, Japan and Singapore as its PoPs, while the store also sells a Los Angeles Pulse line, which the homepage never mentions.

Turin is the flagship line: newer silicon, larger disks, positioned for latency-sensitive and high-concurrency work. Pulse is the balanced line, aimed at blogs, dev environments, small web apps, APIs and proxy nodes. Every plan on the Hong Kong Turin store page carries a "China Mainland Optimized Pro" tag, and GoMami's Pulse documentation lists triple-network optimisation as a line-wide feature. The Pro and Ultra tiers on both Hong Kong lines are additionally marked Windows-ready.

## Four billing details that change the real cost

Annual billing saves 15–20% versus monthly, per GoMami's own documentation. On the $39 EPYC Mini that works out to roughly $374–$398 for a year instead of $468; on the $59 Ryzen Mini, about $566–$602 instead of $708. Both figures are derived from the published discount range, so treat them as estimates and read the number in the cart.

The refund window is 24 hours from activation and applies to first-time purchases only. That's tight by industry standards, where 30 days is common. Renewals, services terminated for ToS violations, spent account credit and IPs blackholed by DDoS are all excluded, and refunds land via Alipay in 1–3 business days, card in 5–10, crypto in 1–3.

There is no grace period. Service is suspended on the due date itself, terminated five days later with data possibly unrecoverable, and the invoice auto-cancels on day six. A renewal invoice is generated seven days ahead and any card on file gets charged automatically. The practical risk is a card that silently expires: you get reminders on days 1, 2 and 3 overdue, then the box is gone.

Traffic is metered outbound only; inbound is free. The quota resets each cycle, and top-up packs (100GB for $6, 1TB for $60, up to 20TB for $1,200) are valid for the current cycle only. They don't roll over. If you find yourself buying a 1TB pack every month, moving up a plan tier makes more sense, since the upgrade also buys CPU and RAM.

👉 [Compare the $39 EPYC and $59 Ryzen entry plans](https://bit.ly/Gomami)

## Who should not buy this

Anyone whose main goal is the cheapest box to learn Ubuntu on. GoMami is a mid-to-high-priced China-route specialist, and its entry point is roughly six times a generic Ubuntu VPS. Prices below were captured in October 2026 and will drift.

| Provider | Entry plan | Specs | Price | Billing note |
| --- | --- | --- | --- | --- |
| Hostinger | KVM 1 | 1 vCPU / 4GB / 50GB NVMe / 4TB | $6.49/mo | Two-year promotional first-term price; renews at $11.99/mo |
| Host4Fun | Spark | 1 vCPU / 1GB / 15GB NVMe / 2TB | $5/mo | Ubuntu 18.04–24.04 templates |
| DropVPS | Starter | 1 core / 1GB / 30GB NVMe / 1TB | $5.90/mo | Intel Xeon hosts |
| VPS Shell | VPS 1 | 1 vCore / 1GB / 20GB NVMe / 8TB | $8/mo | Crypto payments only; 30-day refund |
| GoMami | EPYC Mini | 2 vCPU / 4GB / 40GB NVMe / 1TB | $39/mo | CN2 / 9929 / CMIN2 routing |

The Hostinger row deserves a second look before you use it as a benchmark, because $6.49 is a promotional first-term rate tied to a two-year commitment. At renewal it's $11.99/month, so the true multi-year cost is closer to double the headline.

If your visitors are in Frankfurt or Ohio, four rows in that table beat the fifth on price. The premium buys route quality into mainland China plus the advertised 600 Gbps mitigation figure, neither of which a European hobby project will ever notice.

Where it does make sense: a service with a real Chinese user base, a game server where evening-peak latency is the entire product, an API a Shanghai team hits all day, or a checkout page where a two-second stall costs orders. In those cases $39 isn't the expensive option. A $6 box that's unusable between 8pm and midnight is.

## Placing the order without fumbling it

Log in, pick the node and series in the left sidebar, choose a plan and hit Order Now. The configure page is where you set the billing cycle; the cart's Review & Checkout step is where the promo code field lives, after the plan is chosen and before payment. Payment options are credit card, Stripe Alipay, crypto, or account credit.

Deployment runs automatically after payment and typically finishes within a few minutes, with the IP and root credentials arriving by email. First SSH connection asks you to confirm the host fingerprint before accepting the password, which is normal for any fresh server.

Changing your mind about the OS later means a rebuild from the control panel. That replaces what's on the system disk, so back up anything you care about first. There's also a RESTful API if you'd rather script rebuilds, ISO mounts or template listings than click through a panel.

## FAQ

**Which Ubuntu version should I pick on a VPS?**
24.04 LTS for almost everything. It has standard support into 2029, and it's what package maintainers and Docker image authors test against first.

**Can I run Ubuntu 26.04 LTS on GoMami?**
Not from the template list, which stops at 24.04 LTS. You can mount the 26.04 ISO manually and install through VNC; just remember to unmount the ISO and restore the Hard Disk boot order afterwards.

**Does traffic count uploads and downloads?**
Outbound only. Inbound traffic isn't metered, and the quota resets at the start of each billing cycle.

**Is there a trial?**
Nothing in GoMami's documentation describes one. The documented safety net is the 24-hour full refund on a first purchase, short enough that you should decide what you're testing before you order.

**Can I switch from monthly to annual later?**
GoMami's documentation says the billing cycle can be changed at renewal, and that changing it sooner requires contacting support. Check what the billing page actually offers on your account before assuming either path.

## Picking your line

If you just need a working Ubuntu box and your users aren't in China, close this tab and buy a $6 plan somewhere. Nothing here beats that on price, and pretending otherwise wastes your money.

If China latency is the actual problem, work backwards from RAM. A blog or small API fits the $39 EPYC Mini. Anything running containers with a database alongside wants the $79 Air. Single-core-sensitive work like game servers should start at the $59 Ryzen Mini and ignore the EPYC line entirely. Skip Ultra and Titan unless you already know why you need 32GB.

Two things to check before you pay. First, confirm the disk size on the order page matches what you expected, because the homepage and the store disagree on that number for identically named tiers. Second, decide between monthly and annual: the 15–20% saving is real, but so is the no-grace-period suspension, so only prepay a year if the card on file will still work in eleven months.

👉 [Start with the GoMami plan you picked and select the 24.04 LTS template at deploy](https://bit.ly/Gomami)
