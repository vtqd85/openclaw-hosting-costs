# OpenClaw Hosting: Which Plan You Actually Need, What a Year Really Costs, and Where Self-Managed Stops Making Sense (Plan Table + the Root, Docker and Storage Minimum)

OpenClaw puts three requirements on a server before anything else matters: root-level control of the system layer, Docker, and storage that survives a restart. Meet them and 2 vCPU with 4 GB of RAM will carry one agent across a few channels; miss them and extra RAM buys nothing. AVA Hosting sells OpenClaw in two shapes, a managed application that starts at €5 on Plan 2 and a self-managed Ubuntu 22.04 template that ships with Docker and a web gateway already configured. What follows is the plan table, the yearly cost measured against a Raspberry Pi and a $12 VPS, and the work that is still yours after the install finishes.

> **The short answer for most readers:** OpenClaw 2 on Plan 2 at €5. One-click deployment, a custom domain, automatic HTTPS, automatic application updates and scheduled backups are included, and your data stays attached to the instance. Choose the Ubuntu 22.04 template instead if you need root, custom skills or MCP connectors and are willing to own OS updates yourself.
> 👉 [See AVA Hosting's current OpenClaw 2 plans and pricing](https://my.ava.hosting/aff.php?aff=15562)

## What AVA Hosting actually sells for OpenClaw

AVA Hosting offers two delivery models rather than one plan ladder with different RAM sizes. The managed application is the default; the VPS template is for people who want the machine.

| Option | What you get | Price | Best for | Order |
| --- | --- | --- | --- | --- |
| OpenClaw 2, hosted app on Plan 2 | One-click deployment, custom domain, automatic HTTPS, automatic app updates, scheduled backups, data attached to the instance | €5 on Plan 2 | One agent, a few channels, no server admin | [Start with Plan 2](https://my.ava.hosting/aff.php?aff=15562) |
| OpenClaw 2 on a larger plan | Same managed stack with more resources behind it | Current tier price at checkout | Many concurrent users, heavy jobs, larger datasets | [See the larger tiers](https://my.ava.hosting/aff.php?aff=15562) |
| OpenClaw 1 (legacy), hosted app on Plan 2 | Legacy build with the same managed features | €5 on Plan 2 | Keeping an existing deployment compatible | [View the legacy option](https://my.ava.hosting/aff.php?aff=15562) |
| Ubuntu 22.04 OpenClaw template | OpenClaw, Docker, Docker Compose and the WebGateway preinstalled, firewall rules configured, SSH and root are yours | VPS plan price; 2 GB RAM is the recommended minimum | You want root, custom skills, your own stack | [Pick a VPS plan for the template](https://my.ava.hosting/aff.php?aff=15562) |

AVA Hosting states that larger plans exist for resource-heavy workloads, large datasets and many concurrent users. Each tier's current price and its billing term are shown at checkout.

Three things sit outside that price:

- **Model costs are separate.** OpenClaw needs an existing Claude Code or Codex CLI login, or a compatible provider API key, and you pay that provider directly.
- **Application email is disabled** across hosted apps unless you configure a supported external mail path.
- **The legacy build is for compatibility.** AVA Hosting's own guidance is that new deployments should start on OpenClaw 2.

## The three requirements OpenClaw puts on a server

Root access matters because plugins, ClawHub skills and MCP connectors install at the system level. A failed install has to roll back cleanly, and an update should start with a look at what is already installed. Bluehost's documentation lists exactly these reasons, and it is the most concrete public explanation of the constraint.

Docker is the documented runtime. Without container support you are off the supported path, which is why the AVA Hosting template ships Docker and Docker Compose preinstalled rather than asking you to add them.

Persistent storage covers sessions, skills, workspace files and automation state. On shared hosting, root and Docker are normally blocked and there is no persistent volume, so skills and run history reset on restart. Bluehost calls a self-managed VPS the minimum viable environment for that reason.

On size, the published guidance disagrees in a way that is worth understanding rather than averaging. Bluehost's engineering guide sets the functional minimum at 2 vCPU, 4 GB DDR5 and 100 GB NVMe, and says OpenClaw stops running reliably below that once several channels and automations are running. A separate self-hosting guide takes a lower view, treating a 4 GB old laptop or an 8 GB Raspberry Pi 5 as adequate for light personal use, with 4 GB+ RAM plus Ubuntu 22.04 or Debian 12 as the general Linux floor. Both describe real workloads: one channel and a couple of scheduled jobs sit at the low end; five channels and a dozen automations sit at the other. AVA Hosting's template guide recommends 2 GB RAM as the minimum, which places its entry point on the light-load side of that range. If you plan to connect Slack, Telegram and WhatsApp and run automations around the clock, buy above the minimum.

## Four products sold as "OpenClaw hosting"

**Self-managed VPS with OpenClaw preinstalled.** Contabo's version is the clearest example: pick a Cloud VPS, it arrives with OpenClaw installed, you connect over SSH, run the onboarding wizard, add provider keys and connect messaging apps. The one-click add-on covers the initial installation only. Upgrading and troubleshooting the open-source software stays with you.

**Managed application layer.** AVA Hosting's OpenClaw 2 runs as a hosted application. They apply application updates, take scheduled backups, issue HTTPS for the domain you point at it, and keep configuration and data attached to the deployment. You still own application settings, connected third-party accounts, users and access rules.

**Container or serverless.** A community Cloudflare template runs OpenClaw inside a Sandbox container. Billing splits in two: CPU is charged on active usage only, while memory and disk are charged on provisioned capacity for the whole time the container is running. That template's README estimates $5–6 a month in compute for a container that runs four hours a day, on top of the $5 plan fee, and reaching that number requires setting `SANDBOX_SLEEP_AFTER` so the container sleeps when idle. Deployment is the awkward part: Access policies, a bypass rule for the `/cdp` path, secret naming, container lifecycle and R2 setup all fail quietly when they are wrong.

**Home hardware.** An 8 GB Raspberry Pi 5 is the popular pick, a Mac Mini makes sense if you want iMessage and Apple Notes integration, and an old laptop costs nothing if reliability is not the priority.

The real question is how much of the machine you want to touch. Self-managed VPS means you touch the OS and the application. The managed app means you touch the application. The Cloudflare route means you touch configuration and identity. Home hardware means you are the uptime, the ISP and the person who notices the fan stopped.

## What a year actually costs

Monthly sticker prices across these options are not comparable. Different terms, currencies and billing dimensions. Annual cost is the number that sorts them.

| Setup | Hardware | Power | Hosting | First year | Year 2 onward |
| --- | --- | --- | --- | --- | --- |
| Raspberry Pi 5, 8 GB + 128 GB SSD | $120 | ~$1/mo | $0 | $372 | ~$252/yr |
| Mac Mini M4, 16 GB | $599 | ~$3/mo | $0 | $851 | ~$276/yr |
| VPS at $12/mo | $0 | $0 | $12/mo | $504 | $504/yr |

Figures come from a third-party self-hosting cost model that assumes $25 a month in model API spend, in a $20–40 range. Its conclusion is that self-hosted hardware pays for itself in 6–12 months against a mid-range VPS.

Contabo's headline of €4.40 a month needs reading before you annualise it. That is the average over the first 24 months including applicable taxes, on a Cloud VPS 4 with 4 vCPU, 8 GB RAM and 100 GB SSD. The page states a minimum initial term of one month and a minimum subsequent term equal to the initial one, and it does not publish the price from month 25. Multiply €4.40 by twelve and you have assumed a renewal price nobody has quoted.

The Cloudflare figure only holds with sleep configured. Leave the container up and memory and disk bill around the clock.

AVA Hosting's €5 on Plan 2 is the smallest number in this comparison. When model calls run $20–40 a month, hosting is the line item you should stop optimising. Spend the attention on sizing the box correctly instead, since an undersized plan shows up as failed runs and retries, which bill at API rates.
👉 [Check what Plan 2 includes and what the next tier costs](https://my.ava.hosting/aff.php?aff=15562)

## After install: the work that is still yours

Approvals in OpenClaw bind to the exact request, command, session and person that granted them, and cannot be replayed outside that context. Removing or re-pairing a device revokes its old access immediately. Role-based access controls who can create and change Automations.

Beyond that, the standard hardening list applies regardless of who hosts you: key-based SSH with root login disabled, a strict firewall, a non-root user for OpenClaw, automatic security updates, and Tailscale or a Cloudflare Tunnel rather than port forwarding. Backups deserve a restore test, not just a green checkmark.

One detail specific to the AVA Hosting template: the onboarding wizard is reached at your VPS IP on port 18789 over plain HTTP. Attach a hostname and TLS before you connect channels you actually care about, and change the admin credentials the wizard creates.

## OpenClaw hosting FAQ

**Will several messaging channels slow it down?**
CPU and disk I/O determine that more than channel count does. Every channel connection and every automation run draws on the same network throughput and the same disk, and that is where growth shows up first.

**Do automations survive a server restart?**
OpenClaw logs three things separately for each run: whether it fired, whether it was delivered, and whether it completed. Work missed during a restart resumes without re-running jobs that already finished. The dependency is infrastructure that restarts predictably and comes back quickly.

**Can I run a local model?**
Ollama is on the supported list, and AVA Hosting lists local models alongside Claude and ChatGPT. Running the model and the agent on the same small plan is a different resource class from calling an API, so check the RAM ceiling before you commit to that setup.

**Can several people share one host?**
Yes. Approvals are bound per person and per session, and role-based access limits who can create or modify automations.

**Will it run on shared hosting?**
No. Root, Docker and persistent volumes are normally all unavailable, and skills plus run history reset on restart.

## Which plan to pick

One agent, a few channels, no interest in SSH: OpenClaw 2 on Plan 2 at €5, with AVA Hosting handling updates, backups and HTTPS.

You need root, custom skills, MCP connectors or your own Docker stack: the Ubuntu 22.04 OpenClaw template on a VPS plan, 2 GB RAM minimum and more if several channels will be live.

Five channels and ten automations: size up before ordering. The failure mode on an undersized box is unreliability, not a clean error message.

Keeping an older deployment alive: OpenClaw 1 (legacy) exists for that. New installs should start on OpenClaw 2.

Two things to confirm before checkout. First, the billing term and renewal price for the tier you are choosing, since €5 is the entry tier's listed price and the term is shown at checkout. Second, whether the hosted application gives you the level of access your skills and connectors need. If it does not, the VPS template does, and you trade managed updates for root.
👉 [Open AVA Hosting's OpenClaw 2 page and confirm the current price](https://my.ava.hosting/aff.php?aff=15562)
