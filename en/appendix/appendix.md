# Appendix

This section contains supplementary information that is useful for reference but falls outside the main learning path of the book.

---

## Appendix A: Quotas and Pricing

Gemini CLI offers a generous free tier, but for more intensive use, it's important to understand the quotas and pricing, which vary based on your authentication method.

### Free Usage Tiers
- **Login with Google (Gemini Code Assist for Individuals):** Your usage is covered by a free tier with significant daily and minute-based request limits, using a range of Gemini models.
- **Login with Gemini API Key (Unpaid):** The free tier for API keys has lower request limits and is typically restricted to Flash models.
- **Login with Vertex AI (Express Mode):** Provides a 90-day trial period without needing to enable billing.

### Paid Tiers
- **Fixed Price (Gemini Code Assist Standard/Enterprise):** If you need higher, predictable limits, you can subscribe to a paid Gemini Code Assist plan. This provides a higher number of daily requests for a fixed monthly cost per user.
- **Pay-As-You-Go (Gemini API or Vertex AI):** For the most flexibility and to avoid being rate-limited, you can use a Gemini API key or Vertex AI with billing enabled. In this model, you pay per token used. This is ideal for professional or automated use cases but requires you to be mindful of your usage to avoid unexpected costs.

You can check your usage for the current session at any time with the `/stats` command.

---

## Appendix B: Telemetry and Data Privacy

Understanding how your data is handled is crucial. This section covers the CLI's telemetry feature and the privacy policies that apply to your code.

### Telemetry (Observability)
Gemini CLI includes an optional telemetry feature built on the industry-standard OpenTelemetry framework. When enabled, it can collect anonymous data about CLI usage, performance, and errors. This helps the development team improve the tool.

This feature is **disabled by default**. You can enable it and configure it to send data to Google Cloud or a local file for your own analysis by editing your `settings.json` file.

### Privacy and Use of Your Code
A key question for any developer is: **"Is my code used to train Google's models?"** The answer depends entirely on how you authenticate.

- **YES, your code may be used for training if you log in with:**
  - A **personal Google account** (Gemini Code Assist for Individuals).
  - A Gemini API key on the **unpaid tier**.

- **NO, your code is treated as confidential and is NOT used for training if you log in with:**
  - A **Google Workspace account** (Gemini Code Assist Standard or Enterprise).
  - A Gemini API key on a **paid tier**.
  - A **Google Cloud (Vertex AI)** account.

For enterprise and professional users, your data is kept private. For free-tier individual users, your data may be used to improve the service. You can find the specific Terms of Service and Privacy Notices that apply to your account type on Google's official documentation pages.

---

## Appendix C: Uninstalling Gemini CLI

How you uninstall the CLI depends on how you installed it.

### If you installed with `npm install -g`
This is the most common method. Use the npm uninstall command with the `-g` flag.
```bash
npm uninstall -g @google/gemini-cli
```

### If you used `npx`
`npx` runs packages from a temporary cache without a permanent installation. To "uninstall" it, you need to clear this cache.

- **On macOS / Linux:**
  ```bash
  rm -rf "$(npm config get cache)/_npx"
  ```
- **On Windows (PowerShell):**
  ```powershell
  Remove-Item -Path (Join-Path $env:LocalAppData "npm-cache\_npx") -Recurse -Force
  ```
This will remove all packages previously run with `npx`, not just `gemini-cli`.
