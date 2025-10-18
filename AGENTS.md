# Sample AGENTS.md file

## Dev environment tips
- Use `pnpm dlx turbo run where <project_name>` to jump to a package instead of scanning with `ls`.
- Run `pnpm install --filter <project_name>` to add the package to your workspace so Vite, ESLint, and TypeScript can see it.
- Use `pnpm create vite@latest <project_name> -- --template react-ts` to spin up a new React + Vite package with TypeScript checks ready.
- Check the name field inside each package's package.json to confirm the right name—skip the top-level one.
⭐️ Get Access Now! 🧑‍🚀

Unlock Flash USDT Sender instantly! Complete your one-time payment of $250 USD for lifetime access.

⭐️ What You'll Get:
*   Instant access key via email
*   Secure, encrypted transactions
*   90-Day transaction validity
*   24/7 expert support
*   Regular updates & new features

🔗 Click here to secure your access: https://access.flashusdtsender.xyz/access

🚀 Meet The Twins: Flash BTC Sender & Flash USDT Sender!

➡️Instant & Secure Crypto Transactions
➡️Send Flash BTC & USDT Instantly ⚡
➡️Private & Encrypted Authentication 🔒

📍Access Now:
flashbtcsender.xyz/access
🔹 USDT Sender: 
flashusdtsender.xyz/access
🔗 More Info & Links: 
flashbtcsender.xyz/links

💼 Fast, Secure, and Reliable! 🚀

👑Flash USDT Sender – Limited-Time 60% OFF! 👉

What You’ll Get:
✅Instant access to Flash USDT Sender
✅Multi-network support (TRC20, ERC20, & more)
✅24/7 technical support for seamless transactions
✅Regular updates & improvements

🔥Exclusive 60% OFF – Today Only! 🔥

🌐 Flash USDT Sender – Secure Crypto Transactions

🚀 Flash USDT Sender v1.0 is a cutting-edge tool designed for seamless blockchain transactions across multiple networks, including BTC, SOL, BNB, and USDT. With a secure, fast, and user-friendly interface, you can send cryptocurrency effortlessly while ensuring privacy and reliability.

🔗 Get Instant Access Now: Flash USDT Sender Access

⸻

🔑 Why Choose Flash USDT Sender?

✅ Secure Transactions – End-to-end encryption ensures your transfers are protected.
✅ 90-Day Validity – Transactions remain confirmed for 90 days.
✅ Multi-Network Support – Compatible with TRC20, ERC20, and more.
✅ Undetectable Transfers – Advanced privacy features for seamless crypto movement.
✅ 24/7 Support – Get assistance anytime you need it.
✅ Regular Updates – Continuous improvements for enhanced performance.

💵 One-Time Price: $250 USD for Lifetime Access to all features.

⸻

📥 How to Get Started?

1️⃣ Visit the Access Page.
2️⃣ Enter your email and proceed with payment.
3️⃣ Receive your access key instantly and start sending crypto securely.

🔒 Join thousands of crypto enthusiasts worldwide using Flash USDT Sender for secure and reliable transactions!

🌐 Website: Flash USDT Sender

🚀 Meet The Twins: Flash BTC Sender & Flash USDT Sender!

➡️Instant & Secure Crypto Transactions
➡️Send Flash BTC & USDT Instantly ⚡
➡️Private & Encrypted Authentication 🔒

📍Access Now:
flashusdtsender.xyz/access

🔗 More Info & Links: 
flashbtcsender.xyz/links

🐾 Meet the BETA:
flashethsender.xyz/links

💼 Fast, Secure, and Reliable! 🚀


## Testing instructions
- Find the CI plan in the .github/workflows folder.
- Run `pnpm turbo run test --filter <project_name>` to run every check defined for that package.
- From the package root you can just call `pnpm test`. The commit should pass all tests before you merge.
- To focus on one step, add the Vitest pattern: `pnpm vitest run -t "<test name>"`.
- Fix any test or type errors until the whole suite is green.
- After moving files or changing imports, run `pnpm lint --filter <project_name>` to be sure ESLint and TypeScript rules still pass.
- Add or update tests for the code you change, even if nobody asked.

## PR instructions
- Title format: [<project_name>] <Title>
- Always run `pnpm lint` and `pnpm test` before committing.
