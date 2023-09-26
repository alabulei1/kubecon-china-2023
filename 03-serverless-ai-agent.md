# Build a PR Review bot in a serverless way

## Prerequisites

You will need to bring your own [OpenAI API key](https://openai.com/blog/openai-api). If you do not already have one, [sign up here](https://platform.openai.com/signup).

You will also need to sign into [flows.network](https://flows.network/) from your GitHub account. It is free.

### 1 Create a bot from a template


### 1 Create a bot from a template

[**Just click here**](https://flows.network/flow/createByTemplate/Summarize-Pull-Request)

Review the `trigger_phrase` variable. It is the magic words you type in a PR comment to manually summon the review bot.

Click on the **Create and Build** button.

### 2 Add your OpenAI API key

You will now set up OpenAI integration. Click on **Connect**, enter your key and give it a name.

Close the tab and go back to the flow.network page once you are done. Click on **Continue**.

### 3 Configure the bot to access GitHub

Next, you will tell the bot which GitHub repo it needs to monitor for upcoming PRs to review.

* `github_owner`: GitHub org for the repo *you want to deploy the 🤖 on*.
* `github_repo` : GitHub repo *you want to deploy the 🤖 on*.

> Let's see an example. You would like to deploy the bot to review code in PRs on `WasmEdge/wasmedge_hyper_demo` repo. Here `github_owner = WasmEdge` and `github_repo = wasmedge_hyper_demo`.

Click on the **Connect** or **+ Add new authentication** button to give the function access to the GitHub repo to deploy the 🤖. You'll be redirected to a new page where you must grant [flows.network](https://flows.network/) permission to the repo.

Close the tab and go back to the flow.network page once you are done. Click on **Deploy**.
