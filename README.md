# Hutte Recipe - Browser Automation

> This recipe uses [sfdx-browserforce-plugin](https://github.com/amtrack/sfdx-browserforce-plugin) to set the `Setup > Company Information > Currency Locale` in a Scratch Org using browser automation because this is not supported in the Metadata API.

In this example, we set the Currency Locale to `English (Ireland) - EUR`.

## Prerequisites

- a valid Sfdx Project
- a `hutte.yml` file (e.g. the default one shown in the `CONFIGURATION` tab)

## Steps

The following assumes that we use the `config` directory to store a configuration file for `sfdx-browserforce-plugin`.

### Step 1

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

### Step 2

- Edit the `hutte.yml` file in your default branch
- Add the following lines to the `push_script`

```yaml
push_script: |
  sfdx --version
  export PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
  echo y | sfdx plugins install sfdx-browserforce-plugin
  export PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser
  sfdx browserforce:apply -f config/change-default-currency-locale.json
  sfdx force:source:push -f --loglevel fatal 1>/dev/null
```

### Step 3

- Create a Scratch Org
- Log in to the Scratch Org
- Verify that `Setup > Company Information > Currency Locale` is set to `English (Ireland) - EUR`
