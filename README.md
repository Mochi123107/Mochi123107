## Hi there 👋

<!--
**houj716-cloud/houj716-cloud** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

name: generate-and-upload-card

on:
  # run automatically every 24 hours
  schedule:
    - cron: "0 */24 * * *"

  # allows to manually run the job at any time
  workflow_dispatch:

  # run on every push on the master branch
  push:
    branches:
      - master

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 5

    steps:
      - name: generate github stats card
        uses: LuciNyan/pixel-profile/action@main
        with:
          outputs: |
            dist/github-stats?username=<username>&screen_effect=false&theme=fuji&dithering=true&hide=avatar
            dist/github-stats-dark?username=<username>&theme=crt
          crt_outputs: |
            dist/github-stats-crt?username=<username>&include_all_commits=true
            
      - name: push cards to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
