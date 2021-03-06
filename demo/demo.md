# Demo

- Introduce problem
  - We have a lot of repos at clever
  ```
  ls | wc -l
  ```
  500+ repos!
  - sometimes we want to make the same conceptual change to a large subset of them
  show: 187 PRs for updating to go 1.8 https://github.com/pulls?utf8=%E2%9C%93&q=is%3Aclosed+is%3Apr+%22go+1.7+1.8%22+org%3AClever+NOT+Revert+
  - we have tools to automate some of this process, but the process still takes
    a matter of days or weeks to complete

- Maybe there's a better way: introducing `microplane`.
  At a high level `microplane` breaks down the process into steps:
  - `init`: decide what repos you want to target
  - `clone`: copy those repos to your computer
  - `plan`: iterate on a script to make changes to all of those repos
  - `push`: open pull requests with your changes
  - `merge`: merge the pull requests

- Example: we have a re-org happening on Monday.
  In every repo we encode what team owns the repo
  https://github.com/search?utf8=%E2%9C%93&q=org%3AClever+path%3Alaunch+eng-apps&type=
  Let's do the re-org right now
  - Copy github search string
    ```
    bin/mp init "org:Clever path:launch eng-apps"
    ```
  - Clone these repos:
    ```
    bin/mp clone
    bin/mp status
    ```
  - Write a small script to update old team ownership the new:
    _(NOTE: This script requires git version 2.7 or greater)_
    ```
    less demo/update-team.go
    bin/mp plan -b eng-reorg -m "Eng Reorg: Update team name in launch.yml" -- go run `pwd`/demo/update-team.go
    ```
  - Open PRs--all of them!
    ```
    bin/mp push -a nathanleiby
    ```
  - See PRs opened
    https://github.com/pulls?utf8=%E2%9C%93&q=org%3AClever+%22eng+reorg%22+is%3Aopen

- Next steps:
  - unleash the guilds
  - automate it even further
  - blog about it (this is a problem lots of companies have)
