# PWA for headless e-commerce
> High performant, intuitive and easy to develop progressive web app to connect with the api of your e-commerce systems API based on [Nuxt.js](https://nuxtjs.org/).

![GitHub package.json version](https://img.shields.io/npm/dm/@hubblecommerce/hubble)
![GitHub contributors](https://img.shields.io/github/contributors/hubblecommerce/hubble-frontend-pwa)
![PWA Shields](https://www.pwa-shields.com/1.0.0/series/classic/solid/gray.svg)

✔ Integrated to Shopware 6 and possible with Magento, Magento 2, xt:Commerce, etc.  
✔ Contains all common pagetypes like: Category, Product Detail, Cart, Checkout, Customer Account, ...)  
✔ Excellent Google Lighthouse results in all audits  
✔ Toolbox / Boilerplate to create your own frontend

## Contribution Guide PWA

**1**: Create issue <br>
Every change to the code and every pull request must be assigned to an issue.
The issue ID created is required for the following steps.

**2**: [Fork](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/fork-a-repo) des
[hubble Repositorys](https://github.com/hubblecommerce/hubble-frontend-pwa) auf eigenen Github Account erstellen.

**3**: Setup NuxtJs
``` bash
npx create-nuxt-app <PROJECT-NAME>
```

**4**: Clone new fork
``` bash
cd <PROJECT-NAME>
mkdir modules
cd modules
git clone https://github.com/<YOUR-ACCOUNT-NAME>/hubble-frontend-pwa.git
```

**5**: Register module to nuxtjs
``` json
// ~/package.json
"devDependencies": { 
    "@hubblecommerce/hubble": "file:modules/hubble-frontend-pwa/@hubblecommerce/hubble"
}
```

``` js
// ~/nuxt.config.js
buildModules: [
    ['@hubblecommerce/hubble']
]
```

**6**: Install dependencies
``` bash
npm install
```

**7**: Edit configs in .env file
```sh
# API
# Define api type:
# possible source parameters are:
# api = hubble Api based on elastic search
# sw = official Shopware 6 API (headless Channel)
API_TYPE          = 'sw | api'
API_SW_ACCESS_KEY = ''
API_BASE_URL      = ''
```

**8**: Install dependencies and launch app in dev mode 
``` bash
npm install
npm run dev
```

**9**: Tracking the original repository as a remote fork <br>
This is especially important to keep the fork up to date to the original repository (upstream).
 ``` bash
git remote add --track master upstream https://github.com/hubblecommerce/hubble-frontend-pwa.git
git fetch upstream
 ```

**10**: Create a new branch for the issue based on the upstream master branch
``` bash
git checkout -b issue#<NUM> upstream/master
```

**11**: Push changes to the code to the fork repository (specify issue ID)
``` bash
git add .
git commit -m "issue#<NUM> my detailed commit message"
git push -u origin issue#<NUM>
```

**12**: Pull Request <br>
Visit [Pull requests von hubble](https://github.com/hubblecommerce/hubble-frontend-pwa/pulls).
You should se an automatic suggestion from Github to make a new pull request from the created branch `issue#<NUM>`. <br>
Important! Specify dev as base branch and NOT master.

## Release History
* 1.3.0
    * Refactored app to improve performance
* 1.2.0
    * Specified API to Shopware 6 store-api
* 1.1.0
    * Extracted the hubble core to nuxt module
* 1.0.0
    * The first proper release

## Meta

digital.manufaktur GmbH – hallo@digitalmanufaktur.com

Distributed under the MIT license. See [LICENSE](https://github.com/hubblecommerce/hubble-frontend-pwa/blob/master/LICENSE.txt) for more information.

[https://github.com/hubblecommerce/hubble-frontend-pwa](https://github.com/hubblecommerce/hubble-frontend-pwa)

## Contributing

Detailed instructions to do pull requests can be found here:
[hubble Contribution Guide](https://docs.hubblecommerce.io/pwa/contribution/contributionpwa.html)

