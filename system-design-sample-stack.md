## Tools and Services I Use to Run a <sample> SaaS

Who doesn’t love a good stack teardown? Ever since I saw [Cushion’s running costs](https://cushionapp.com/running-costs/) page, I’ve been low key addicted to reading about how other apps are built.

I work on an app called [SongRender](https://songrender.com/) that helps musicians and podcasters make videos for social media. So in the spirit of the teardown, here are the tools and services I use to run it!

### Languages

This isn’t an exhaustive list — I’ve omitted some obvious ones (like HTML) and ones that are implied by other tooling choices (like HCL for Terraform).

- [TypeScript](https://www.typescriptlang.org/) — A superset of JavaScript with static types. I fell in love with this language when I first tried it a few years ago, and it’s only gotten better since then. I use it for both the frontend and server-side code.
- [SCSS](https://sass-lang.com/) — A superset of CSS with cool features like mixins and nesting. I use it for both the marketing site and — with [CSS modules](https://github.com/css-modules/css-modules) — the web app.

### Build Systems and Frameworks

- [React](https://reactjs.org/) — The frontend is a single-page app built with React, which has been a great choice — it gets out of the way and lets me just work on the app. There might be other frameworks out there that are quote unquote better, but the sheer size of the React community means I basically never run into any uncharted territory.
- [Create React App](https://create-react-app.dev/) — A batteries-included build system for React. The first time I made a React app, I cobbled together the configuration for Webpack/Babel/etc myself. Create React App hides all that — with the option to “eject” and get the full configuration if I ever need to do something it doesn’t support.
- [Express](https://expressjs.com/) — A Node.js server-side framework. Just like with React, I chose it because it’s the most popular by far. As for why I chose Node.js rather than another language, it’s because I need to run the same video rendering code in the browser and on the server.
- [Hugo](https://gohugo.io/) — A fast static site generator written, used to build the marketing site.

### Libraries

This list is also not exhaustive, but there are too many libraries to name them all here, so I’ve tried to keep it to the most notable or interesting ones.

- [immer](https://immerjs.github.io/immer/docs/introduction) — An intuitive, performant way to do immutability. It can also serialize patches you make to an object, so it’s fairly easy to implement undo functionality as well.
- [downshift](https://www.downshift-js.com/) — A React library for building accessible dropdowns and multi-selects. Easy to style, and you get accessibility for free.
- [popper](https://popper.js.org/) — A nifty little tooltip positioning library.
- [node-canvas](https://github.com/Automattic/node-canvas) — Node library for using the [canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) on the server.
- [ffmpeg](https://www.ffmpeg.org/) — Swiss army knife audio and video library. I use this to on the server convert audio files to WAV and to combine individual frames into users' videos.

### Infrastructure

- [Namecheap](https://www.namecheap.com/) — Domain name registrar. Don’t get turned off by the slightly sketchy name — they’re my favorite registrar that I’ve used.
- [Cloudflare](https://www.cloudflare.com/) — Provides DNS, SSL termination and DDOS protection for the websites and API.
- [Netlify](https://www.netlify.com/) — Hosts static files for the app frontend and marketing site. It’s really easy to use and there’s a generous free tier.
- [DigitalOcean](https://www.digitalocean.com/) — Cloud infrastructure provider, but with plain Linux servers rather than proprietary services. I use them for servers, databases, and object storage to host users' file uploads and videos. Their web UI and API are both really easy to use; I never want to go back to AWS. [💰 Affiliate link](https://m.do.co/c/e506b6f94e20)
- [Postmark](https://postmarkapp.com/) — Transactional email provider. I switched from [SendGrid](https://sendgrid.com/) when I exceeded their free tier and couldn’t get in touch with support, and boy am I glad I did. Postmark’s delivery rate is better, and the dashboard is faster and easier to use. If you’re a bootstrapped startup, they’ll give you a [$75 credit](https://postmarkapp.com/for/bootstrapped-startups).

### Databases

- [PostgreSQL](https://www.postgresql.org/) — Relational database for persisting the app’s data. I briefly considered [MySQL](https://www.mysql.com/) as well, but PostgreSQL has a bunch of really nice features like [returning data from modified rows](https://www.postgresql.org/docs/9.5/dml-returning.html). I did not consider non-relational databases like [MongoDB](https://www.mongodb.com/).
- [Redis](https://redis.io/) — Key/value store, only used for a task queue. Usually I would put this part off and just build a monolith, but CPU-intensive video rendering needs its own server.
- [InfluxDB](https://www.influxdata.com/) — Time-series database for storing metrics. See the section on [monitoring](https://jake.nyc/words/tools-and-services-i-use-to-run-my-saas/#monitoring).

### Servers

- [Ubuntu](https://ubuntu.com/) — The Linux distribution that the servers run. Like other parts of the stack, I chose it because it’s one of the most popular.
- [nginx](https://nginx.org/) — A web server and reverse proxy, used here as a load balancer for the API servers.
- [systemd](https://systemd.io/) — A daemon that manages the processes on the server, like Node.js and nginx. [pm2](https://pm2.keymetrics.io/) is another popular one, but it’s Node-specific.

### Deploying

I’ve previously written about how I deploy SongRender, so you can [read about it in more depth](https://jake.nyc/words/bluegreen-deploys-and-immutable-infrastructure-with-terraform/) if you’re interested. If you just want the high level, these are the tools involved.

- [Terraform](https://www.terraform.io/) — An “infrastructure as code” tool, where you describe the infrastructure you want and it diffs with the infrastructure that actually exists. Kind of like React, but for configuration management. I love Terraform, and I use it to manage any infrastructure it supports.
- [Packer](https://www.packer.io/) — A tool for building machine images. This makes it easy to deploy servers with Terraform.

### Monitoring

- [Sentry](https://sentry.io/organizations/songrender/issues/) — Error tracking. Picks up any uncaught exceptions in the client- and server-side code.
- [Statuscake](https://www.statuscake.com/) — Uptime monitoring, making sure the website and API don’t go down. Chosen for its generous free tier.
- [Papertrail](https://www.papertrail.com/) — Log management. It forwards system log messages using `rsyslog`, so I can just use `console.log` in my server-side code and have it picked up. At some point I might try to switch to a service that supports structured logs, like [Timber](https://timber.io/).
- [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/) — Agent that collects metrics from the servers and forwards them to InfluxDB. It makes it very easy to get metrics on CPU/memory/etc without paying an arm and a leg for something like [Datadog](https://www.datadoghq.com/).
- [Grafana](https://grafana.com/) — Metrics dashboard that displays data from InfluxDB and PostgreSQL. It’s easy to set up and use, and its visualizations are pretty.

### Analytics

- [Indicative](https://www.indicative.com/) — App analytics. I was using [Mixpanel](https://mixpanel.com/), but I ended up exceeding their free tier — which was a shame, because I liked them a lot more. Might check out [Amplitude](https://amplitude.com/) when I have time.
- [Fathom](https://usefathom.com/) — Website visitor stats for the marketing page and app. It’s simpler than Google Analytics and only $15 per month for small sites. [💰 Affiliate link](https://usefathom.com/ref/WBX5G9)
- [Google Search console](https://search.google.com/search-console/about) — Analytics about Google searches for which SongRender has shown up in the results.

### Payments

- [Stripe](https://stripe.com/) — Credit card processing and subscriptions. I **love** using Stripe. Their APIs are so well thought out, the developer tooling is great and their documentation and dashboard are beautiful and easy to use. Their support is also responsive and helpful. I had such a positive experience with them that I registered SongRender LLC with Stripe Atlas, even though it cost a bit more than doing it manually.
- [PayPal](https://www.paypal.com/) — Additional payment option. As happy as I am with Stripe, I’m that unhappy with PayPal: their tooling is lackluster, their documentation is not comprehensive and their website is slow. It’s also clearly not meant as a payment option for in-app purchases — for example, if you’re a new merchant, they’ll hold payments for a few weeks unless you upload a shipping label, which I obviously can’t do. People seem to like having it as a payment option, though.
- [Mercury](https://mercury.com/) — My business bank. One of the three options Stripe recommended as part of Atlas.

### Design

- [Sketch](https://www.sketch.com/) — Vector graphics editor. I designed the SongRender logo using this, and I touch up icons with it too. I tend to go back and forth between this and [Figma](https://www.figma.com/), which I also really like — but there’s still an appeal to using a [native app](https://www.sketch.com/blog/2020/10/26/part-of-your-world-why-we-re-proud-to-build-a-truly-native-mac-app/) that doesn’t store all my data in the nebulous cloud.
- [Nucleo](https://nucleoapp.com/) — Library of SVG icons. I bought the lifetime package years ago and have used it in basically every project since. [💰 Affiliate link](https://nucleoapp.com/?ref=427)
- [SVGOMG](https://jakearchibald.github.io/svgomg/) — Nifty web app that cleans up and compresses SVGs. It’s basically a frontend for [SVGO](https://github.com/svg/svgo).
- [squoosh](https://squoosh.app/) — Nitfy web app that compresses PNGs and JPGs; the raster image counterpart to SVGOMG.

### Development

- [VS Code](https://code.visualstudio.com/) — Not quite a text editor, not quite an IDE. Probably the best Electron app I’ve ever used. I keep checking out [Nova](https://nova.app/) out of a love for all things Panic, but it’s not quite there yet.
- [Postgres.app](https://postgresapp.com/) — Simple free local PostgreSQL server for macOS.
- [dbmate](https://github.com/amacneil/dbmate) — Language- and database-agnostic migration tool.
- [Gitlab](https://gitlab.com/) — Source code hosting and versioning.
- [Prettier](https://prettier.io/) — Code formatter for JavaScript, HTML and CSS. If you’ve never used a code formatter before, do it now. It’ll change your life.
- [Jest](https://jestjs.io/) — Test runner for JavaScript. Bundled with Create React App.
- [Yarn](https://yarnpkg.com/) — Alternative package manager for Node.
- [Make](https://en.wikipedia.org/wiki/Make_(software)) — SongRender doesn’t need to be compiled, so this is just a task runner. [Self-documenting Makefile snippets](https://www.thapaliya.com/en/writings/well-documented-makefiles/) are super helpful.

### Debugging

- [Postico](https://eggerapps.at/postico/) — Great indie Mac app for querying Postgres databases.
- [Insomnia](https://insomnia.rest/) — HTTP client. I use this sometimes when I’m working on the API and don’t want to worry about the browser. I also use it as a very crude admin dashboard: the API has a few admin endpoints for things like retrying a failed render, which I hit directly from Insomnia. [Paw](https://paw.cloud/) is a non-Electron alternative that I’ll spend the $50 on at some point.
- [Transmit](https://www.panic.com/transmit/) — File transfer app. I use this whenever I need to poke around object storage, since it’s much easier than using DigitalOcean’s web-based file browser.

### Support

- [Crisp](https://crisp.chat/) — In-app support chat. It’s the slickest, least obtrusive one I could find with a free tier. I’d like to switch to [Intercom](https://www.intercom.com/) but can’t justify the cost.

### Organization

- [Trello](https://trello.com/) — A kanban board for keeping track of bugs and features. A killer feature for me is the ability to add checklists to cards. Before I deploy a feature, I make an exhaustive checklist of cases to test. I’ve caught so many bugs this way; checklists should be a crucial tool in any developer’s QA arsenal. [Checklists work](https://blog.nuclino.com/the-simple-genius-of-checklists-from-b-17-to-the-apollo-missions)! [💰 Affiliate link](https://trello.com/jakelazaroff/recommend)
- [Notion](https://www.notion.so/) — A repository for less structured note-taking.
- [Fastmail](https://www.fastmail.com/) — The email provider I use [instead of Gmail](https://jake.nyc/words/giving-up-google). [💰 Affiliate link](https://ref.fm/u20086848)

### Marketing

- [Acorn](https://flyingmeat.com/acorn/) — Raster image editor. I mostly use it as a lightweight Photoshop alternative when I need to tweak screenshots.
- [iA Writer](https://ia.net/writer) — Markdown word processor. I use it to write blog posts.

### Retired

Not every relationship was meant to live forever. These are all the tools that I’ve stopped using for one reason or another.

- [Ansible](https://www.ansible.com/) — Provisioning and deployment tool. Replaced by Packer and Terraform.
- [Let’s Encrypt](https://letsencrypt.org/) — Free SSL certificate authority. Replaced by Cloudflare, which does this automatically.
- [Healthchecks](https://healthchecks.io/) — Cron job monitor. Replaced by nothing; I refactored away all my cron jobs. This was pretty useful and I’d sign up again if I needed it.
- [SendGrid](https://sendgrid.com/) — A transactional and marketing email service. Replaced by Postmark, which has a faster web UI and better deliverability.
- [Mixpanel](https://mixpanel.com/) — Product analytics. Replaced by Indicative when I exceeded their free tier and the next tier up was too expensive.
- [Gandi](https://www.gandi.net/) — Domain name registrar. I was a fairly satisfied customer until they followed up a data loss incident (which didn’t affect me, thankfully) with some of the [worst customer service](https://twitter.com/gandi_net/status/1215280538313019396) I’ve ever seen. Replaced by Namecheap.