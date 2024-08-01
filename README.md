# Hutte Recipe - Browser Automation

> This recipe uses [sfdx-browserforce-plugin](https://github.com/amtrack/sfdx-browserforce-plugin) to set the `Setup > Company Information > Currency Locale` in a Scratch Org using browser automation because this is not supported in the Metadata API.

In this example, we set the Currency Locale to `English (Ireland) - EUR`.

## Prerequisites

- a valid sfdx project
- a `hutte.yml` file (e.g. the default one shown in the `CONFIGURATION` tab)

The following assumes that we use the `config` directory to store a configuration file for `sfdx-browserforce-plugin`.

## Step 1: Create currency locale file

Create a `config/change-default-currency-locale.json` file with the following content:

```json
{
  "$schema": "https://raw.githubusercontent.com/amtrack/sfdx-browserforce-plugin/master/src/plugins/schema.json",
  "settings": {
    "companyInformation": {
      "defaultCurrencyIsoCode": "English (Ireland) - EUR"
    }
  }
}
```

## Step 2: Modify push script

- Edit the `hutte.yml` file in your default branch
- Add the following lines to the `push_script`

```yaml
push_script: |
  set -euo pipefail # fail fast
  # install dependencies for Chrome: https://github.com/jontewks/puppeteer-heroku-buildpack/blob/bf058b1bb420204357124941c58d80f8250cc463/bin/compile#L60-L85
  apt-get -qq -y install gconf-service libappindicator1 libasound2 libatk1.0-0 libatk-bridge2.0-0 libcairo-gobject2 libdrm2 libgbm1 libgconf-2-4 libgtk-3-0 libnspr4 libnss3 libx11-xcb1 libxcb-dri3-0 libxcomposite1 libxcursor1 libxdamage1 libxfixes3 libxi6 libxinerama1 libxrandr2 libxshmfence1 libxss1 libxtst6 fonts-liberation > /dev/null
  echo y | sf plugins install sfdx-browserforce-plugin
  sf browserforce apply -f config/change-default-currency-locale.json
  sf project deploy start
```

### Step 3: Validate

- Create a Scratch Org
- Log in to the Scratch Org
- Verify that `Setup > Company Information > Currency Locale` is set to `English (Ireland) - EUR`
