# hf-space-keeper

GitHub Actions workflow that pings a Hugging Face Space on a schedule.

Free Spaces can sleep when nobody visits. This repo sends a regular HTTP request to your Space URL so the Space keeps getting traffic.

## What it does

The workflow is [`.github/workflows/keep_alive.yml`](.github/workflows/keep_alive.yml).

- Runs every 5 minutes (`cron: '*/5 * * * *'`).
- Can also be started by hand from the Actions tab (`workflow_dispatch`).
- Reads the Space URL from the `HF_SPACE_URL` Actions secret.
- Sends a GET request with `curl` and discards the response body.

```yaml
on:
  schedule:
    - cron: '*/5 * * * *'
  workflow_dispatch:
```

## Setup

1. Open this repository on GitHub.
2. Go to **Settings → Secrets and variables → Actions**.
3. Add a repository secret:
   - Name: `HF_SPACE_URL`
   - Value: the direct Space URL, for example `https://your-username-your-space.hf.space`
4. Open the **Actions** tab and allow workflows if GitHub asks.
5. Open **Keep Hugging Face Space Alive** and choose **Run workflow** to test it.

Use the `*.hf.space` app URL, not the `huggingface.co/spaces/...` page. The app URL is the one that hits the running Space.

Do not put the URL in the workflow file. Keep it in the secret.

## Files

| File | Purpose |
| --- | --- |
| `.github/workflows/keep_alive.yml` | Scheduled and manual ping |
| `README.md` | This file |

## Limits

- GitHub can delay scheduled workflows, especially near the start of each hour. A `*/5` cron is not a guaranteed 5-minute timer.
- GitHub turns off scheduled workflows after 60 days with no repository activity. A commit or a manual run counts as activity.
- The workflow prints success even if the request fails. `curl` does not check the HTTP status code.
- A ping does not guarantee that Hugging Face will keep a free Space warm. Hugging Face can still pause a Space.
- Very frequent pings use a lot of Actions runs. If you only need to avoid a long sleep, a slower schedule is enough.

## License

No license file is included yet.
