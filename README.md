# Finding our community members on Mastodon

This repo contains importable CSV files with the Mastodon addresses of Data Platform, PowerShell and Power BI Twitter communities. It has been compiled using **only publicly available** information.

> **Note**
>
>  If you'd like to be included, please add your Mastodon address to your Twitter name, username, bio or pinned Tweet and it'll be added to the CSV sometime in the wee hours of the night GMT.
>
> Please also let others know to do the same!

This project has a lot of possiblities and this is just a preview of what's to come. If you're in the data community and would like to add any additional lists, please do! Also, if you aren't on Mastodon yet and are looking to sign up, we recommend [dataplatform.social](https://dataplatform.social) run by [Daniel Hutmacher](https://dataplatform.social/@dhmacher).

Also, if you're a JavaScript dev, we're [looking for some help](#we-need-help) to improve this project.

# Look how pretty

If you'd like to look at the CSVs now, GitHub makes them pretty and searchable.

* [Data Platform Mastodon List](https://github.com/dataplat/mastodon/blob/main/dataplat/mastodon-import.csv)
* [Power BI Mastodon List](https://github.com/dataplat/mastodon/blob/main/powerbi/mastodon-import.csv)
* [PowerShell Mastodon List](https://github.com/dataplat/mastodon/blob/main/dataplat/mastodon-import.csv)

# Importing your CSV files

There are a number of ways to import these lists to your Mastodon follow list. Here are two.

## Using the web interface

Mastodon has easy importing built right in, so after you've downloaded the appropriate `mastodon-import.csv` file, you can:

Go to Mastodon -> Preferences -> Import & Export -> Import -> Choose your CSV -> Merge

![image](https://user-images.githubusercontent.com/8278033/204133498-1f92850f-0de8-4864-b59d-ccb02b462881.png)

## Using GitHub Actions

You can also auto-import using the Mastodon API and the GitHub Action, [Mastodon Influx](https://github.com/marketplace/actions/mastodon-influx) (or whatever API interface you wish.)

The workflow will essentially look like this.

```yaml
name: Import Twitter Friends to Mastodon
on:
  workflow_dispatch:
  schedule:
    - cron: "30 2 * * *"
jobs:
  check-exodus:
    runs-on: ubuntu-latest
    steps:
      - name: Import CSV to Mastodon
        uses: potatoqualitee/influx@v1
        with:
            server: tech.lgbt
            file-path: https://raw.githubusercontent.com/dataplat/mastodon/main/dataplat/mastodon-import.csv, https://raw.githubusercontent.com/dataplat/mastodon/main/powershell/mastodon-import.csv, https://raw.githubusercontent.com/dataplat/mastodon/main/powerbi/mastodon-import.csv
        env:
            ACCESS_TOKEN: "${{ secrets.ACCESS_TOKEN }}"
```

Check out the [Mastodon Influx](https://github.com/potatoqualitee/influx) docs for more information, including how easy it is to get a Matodon API key.

# How are these lists created?

These lists were built using the [Twitter Exodus GitHub Action](https://github.com/marketplace/actions/twitter-exodus), which makes it easy to find Twitter friends on Mastodon.

For the Data Platform Community, check out the [dataplat.yml](https://github.com/potatoqualitee/exodus/blob/main/.github/workflows/dataplat.yml) workflow file. But basiacally, it checks

```yaml
# Check to see if these Twitter accounts have a Mastodon address yet
specific-twitter-accounts: DataGrillen, SQLBits, SQLServer, AzureSQL
# Look for Mastodon addresses in accounts that follow these accounts
account-followers: DataGrillen, datasaturdays
# Check to see who these accounts follow
accounts-following: DataGrillen, SQLBits, AzureSQL, redgate
# Check the members of these lists
list-members: "1491474973998915587, 1569973251161616385"
# Check the followers of this list
list-followers: "1569973251161616385"
# Check for people who are Tweeting about sqlfamily
hashtags: "#sqlfamily"
# include-private is set to false by default, but this is set to reassure people
include-private: false
```

You can also see how [PowerShell Community](https://github.com/potatoqualitee/exodus/blob/main/.github/workflows/powershell.yml) and the [Power BI Community](https://github.com/potatoqualitee/exodus/blob/main/.github/workflows/powerbi.yml) are checked.

# We need help!

Any JavaScript programmers available? We need to smash the Mastodon CSV files with Twitter CSV files to let people see, select, and export a filtered CSV if they wish. The Twitter CSV contains links to the person's profile pic (along with name, username, etc) so that could be used to make it easier for people to identify their Twitter friends.

It would be even cooler to allow them to specify their Mastodon server then automatically make Follow buttons that let them just go down the list and add who they want with the click of a button.

Maybe it could even ask for their API and check to see if they're already friends and that follow button would be grayed out.

We could put all of this online using GitHub Pages.
