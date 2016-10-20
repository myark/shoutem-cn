---
layout: doc
permalink: /docs/extensions/tutorials/3rd-party-services-integration
title: 3rd party services integration
section: Tutorial
---

# Integrating with 3rd party services

This tutorial shows you how to integrate extension with 3rd party service, such as Shopify. We're going to write our own little service that will communicate with Shopify and provide Shoutem with API token to access 3rd party service data.

> #### Note
> This tutorial includes knowledge from [Getting started]({{ site.baseurl }}/docs/extensions/getting-started/introduction) and [Writting a settings page]({{ site.baseurl }}/docs/extensions/tutorials/writing-settings-page) so make sure to pass them before.

## Shopify application

Before describing what we're going to build, these underline concepts need to be understood:

- Shopify App & Shopify Store
- Shoutem App & Shopify Extension

We're building ***Shoutem*** _extension_ which will be used in ***Shoutem*** _apps_. ***Shoutem*** _extension_ will use **your** ***Shopify*** _app_ to have authorized access over ***Shopify*** store of application owners that installed ***Shopify*** _extension_ to their ***Shoutem*** _application_. When talking about _application owners_, we think of Shoutem app.

For purpose of this tutorial, you will need to become [Shopify Developer](https://developers.shopify.com) (type of Shopify Partner) to create Shopify app. Once you've [created account](https://app.shopify.com/services/partners/signup/developer), creating Shopify app is pretty straightforward. Read [Shopify's documentation](https://docs.shopify.com/api/guides/introduction) on how to create your ***Shopify*** _app_. Enter app's name, leave the default settings and for App URL and Redirect URL, choose `http://localhost`.

## About the extension

Shopify extension will enable application owners, to sell products from their Shopify store to users of their app. Owners will need to authorize your Shopify app to get products from owners' store. Users will be able to put products to the basket and make a purchase. 

## Creating UI

Let's start by creating extension UI. This is how the final extension should look like:

[Picture]
<p class="image">
<img src='{{ site.baseurl }}/img/tutorials/shopify/showcase.jpg'/>
</p>

Initialize extension:

```ShellSession
$ mkdir Shopify && cd Shopify
$ shoutem init shopify
Enter information about your extension. Press `return` to accept (default) values.
Title: (Shopify)
Version: (0.0.1)
Description: Let users buy products from your Shopify Store!

Extension initialized!
```

We're going to need 4 screens:

- List of products
- Details of products
- Add to card
- Checkout

For example of selling clothes, we create static assets that you can use as mock data. [Download](/docs/coming-soon) those assets and put them to `app/assets` folder. Add shortcut and assign it first list screen.

```ShellSession
$ shoutem shortcut add products --with-screen=ProductsList
Enter shortcut information.
Title: Products
Description: List products and enable users to buy them.
```

This will create a shortcut, screen and reference in the shortcut. This is the extension.json that you should have by now:

```JSON
#file: extension.json
{
  "name": "shopify",
  "version": "0.0.1",
  "title": "Shopify",
  "description": "Let users buy products from your Shopify Store!",
  "shortcuts": [{
    "name": "products",
    "title": "Products",
    "description": "List products and enable users to buy them.",
    "screen": "@.ProductsList"
  }],
  "screens": [{
    "name": "ProductsList"
  }]
}
```

Implement list screen of products from assets using [Shoutem UI Toolkit](http://shoutem.github.io/docs/ui-toolkit/introduction).

```JavaScript
import React from 'react';
import {
  Image,
  ListView
} from '@shoutem/ui';

export default class ProductList extends React.Component {
  getProducts() {
    return require('../assets/data/products.json');
  }

  renderRow(product) {
    <Row>
      <Image
        styleName="small"
        source={{ uri: product.image && product.image.url ? product.image.url : undefined }}
      />
      <View styleName="vertical stretch space-between">
        <Subtitle>{{ product.title }}/Subtitle>
        <View styleName="horizontal">
          <Subtitle styleName="md-gutter-right">{{ product.price }}</Subtitle>
        </View>
      </View>
      <Icon styleName="disclosure" name="right-arrow" />
    </Row>
  }

  render() {
    this.props.setNavBarProps({
      rightComponent: (<Button styleName="right-icon">
        <Icon name="add-to-cart" />
      </Button>)
    });

    return (
      <ListView
        data={this.getProducts()}
        renderRow={product => this.renderRow(product)}
      />
    );
  }
}
```

Push extension and install it onto new app:

```ShellSession
$ shoutem install --new ShopifyDemo
Extension is installed onto newly created `ShopifyDemo` application.
See it in browser: `https://builder.shoutem.com/apps/52634`
```

Before creating the rest of the UI, let's authorize shopify app.

## Store settings

Create shortcut settings page where application owners will enter their store address.

```ShellSession
$ shoutem page add StoreSettings
File `server/pages/StoreSettings.js` was created!
```

Reference page in shortcut.

```JSON{7-10}
#file: extension.json
  ...
  "shortcuts": [{
    "name": "products",
    "title": "Products",
    "description": "List products and enable users to buy them.",
    "screen": "@.ProductsList",
    "settingsPages": [{
      "page": "@.StoreSettings",
      "title": "Store"
    }]
  }],
  ...
```

And implement it.

```JavaScript
#file: server/pages/StoreSettings.js
import React from 'react';
import {
  connect
} from 'react-redux';
import {
  FormGroup,
  ControlLabel,
  FormControl,
  Button
} from 'react-bootstrap';
import {
  setShortcutSettings
} from '@shoutem/builder-sdk'

class StoreSettings extends React.Component {
  constructor(props) {
    super(props);

    this.onButtonClick = this.onButtonClick.bind(this);
  }

  onButtonClick() {
    // TODO
  }

  render () {
    const { settings } = this.props.shortcut;

    return (
      <form>
        <FormGroup>
          <ControlLabel>Insert the title of header:</ControlLabel>
        </FormGroup>
        <FormControl
          type="text"
          ref="headerTitle"
          placeholder="Header title"
        />
        <Button
          onClick={this.onButtonClick}
          type="submit"
        >
          AUTHORIZE
        </Button>
      </form>
    );
  }
}

export connect(undefined, {
  setShortcutSettings
})(StoreSettings);
```

Actual authorization logic is still to be implemented

## Authorization

On Shopify, you can create either public or private app. We're going for public, as private one would be restricted only for one store. Your Shopify app will retrieve data from an application owner's store, once the owner authorizes your Shopify app. Shopify has a [document](https://help.shopify.com/api/guides/authentication/oauth) explaining how their authorization works. Authorization is based on _OAuth 2.0 specification_ which is followed on many other 3rd party service. Hence, we'd like to outline underlying concepts a bit better:

- **API KEY** <br />
  Each Shopify App that you create has API KEY and API SECRET. API KEY is used to identify Shopify app and doesn't need to be kept secret.

- **API SECRET** <br />
  Used with API KEY to authenticate Shopify app. It shouldn't be revealed to anyone.

- **ACCESS TOKEN** <br />
  A piece of information used to access specific store through Shopify app. Can be shared with application owner.

- **SCOPE** <br />
  Specifies what Shopify app will be allowed to do on Shopify store. Check which [scopes](https://docs.shopify.com/api/guides/authentication/oauth#scopes) Shopify defines.

### Authorization Lifecycle

1. Owner enters URL of the shop on the shortcut settings page and clicks on "Authorize"
2. Owner is sent to Shopify by server side of extension, which is requesting authorization by sending:
  - `API KEY`
  - `SCOPE`
  - `redirect URL` on which Shopify will redirect once owner authorizes the app, namely extension server
  - `nonce`, random value, which should be sent back from Shopify
3. Once app is authorized, Shopify sends owner to `redirect URL`, extension server
4. Extension server is sending `API KEY` and `API SECRET` to Shopify to grant `ACCESS TOKEN`
5. Shopify responds with the `ACCESS TOKEN` which will be used for that shop
6. Extension server saves the `ACCESS TOKEN` to shortcut settings
7. Extension server sends owner to shortcut settings page

Notice the mention of `extension server`. To make this extension work, we're going to need to write simple server that will hold authorization logic for Shopify and `API SECRET`. 

Worry not! We've prepared a [document](TODO) and [skeleton project](TODO) for that purpose. Follow that document and you will have your own server running in 5 minutes on your local machine through [ngrock](TODO).

Read in [Shopify docs](TODO) how the URL should be structured. Create now `server/assets/js/authorization.js` file and add following lines:

```
var EXTENSION_HOST = "[YOUR_HOST]"
var API_KEY = "[YOUR_API_KEY]";
var SCOPE = "read_products";
var REDIRECT_URL = EXTENSION_HOST + "/shopify/auth/" + SHOUTEM_APP_ID;

$('#settingsForm').on('submit', function(e) {
  var shop = $('#shop').val();

  var installUrl = "http://" + shop + ".myshopify.com/admin/oauth/authorize?client_id=" + API_KEY + 
    "&scope=" + SCOPE + "&redirect_uri=https://" + REDIRECT_URL;

  // TODO - rather open modal or something similiar
  // Go to shopify to authorize
  window.location.replace(installUrl);
});

```

where you should replace [YOUR_HOST] with your localhost URL and [YOUR_API_KEY] with the API_KEY of Shopify app. Notice that we have one unresolved variable: `SHOUTEM_APP_ID`. Leave that for now, until we introduce Shoutem SDK.

Following the authorization lifecycle, let's add a _route handler_ on our server which will use Shoutem API.










































