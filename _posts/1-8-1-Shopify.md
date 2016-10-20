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

We're building ***Shoutem*** _extension_ which will be used in ***Shoutem*** _apps_. ***Shoutem*** _extension_ will use **your** ***Shopify*** _app_ to have authorized access over ***Shopify*** store of application owners that installed ***Shopify*** _extension_ to their ***Shoutem*** _application_.

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

## Authorization

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

Actual implementation










































