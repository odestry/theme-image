# Theme Image

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat&colorA=338fbb&colorB=1c1c1c&logoColor=ffffff)](https://github.com/odestry/.github/blob/main/CONTRIBUTING.md)
[![CI](https://img.shields.io/github/actions/workflow/status/odestry/theme-image/ci.yml?style=flat&label=CI&colorA=338fbb&colorB=1c1c1c&logoColor=ffffff)](https://github.com/odestry/theme-image/blob/main/.github/workflows/ci.yml)
[![Discord Shield](https://img.shields.io/discord/983602196493004820?style=flat&colorA=338fbb&colorB=1c1c1c&label=discord&logo=discord&logoColor=ffffff)](https://discord.gg/blanklob-community-983602196493004820)

[Decisions](#decisions) |
[Usage](#usage) |
[Tools](#tools) |
[Contributing](#contributing) |
[License](#license)

An attempt to create the perfect image snippet for Shopify storefronts.

## Decisions

The goal was to create something that is powerful out of the box without sacrificing **simplicity**, and that works seamlessly with native Shopify Image APIs like focal point.

Another decision was to make it simple to use but also easy to extend for developers with specific needs.

When used, the image will always output a placeholder that is configurable via the params and is the same as the image if there is one. This allows for better consistency.

### Features

- Supports Shopify Focal points.
- Supports native browser features like lazyloading and async decoding.
- Outputs the latest Shopify placeholders with an image tag if the image is empty, for better consistency.
- Built for performance with no layout shift and lower LCP as possible.
- Responsive by default.
- Extremely easy to use and comes with a documented sample API.

## Usage

### Arguments

The image has a very easy-to-understand sample API, and all of these arguments are optional:

- `image`: The image object. This can be a product image or a media object of type image as well.
- `class`: A string of classes, same as the class HTML attribute.
- `lazyload`: By default, the snippet is eagerly loaded. If you want to lazyload the image, you can set this boolean parameter to true.
- `width`: This is the maximum width of the srcset. In some cases, it makes sense to load a smaller srcset if the image is small.
- `alt`: You can set a custom alt text. By default, Shopify uses the product title as the alt text if the image is associated with a product.
- `placeholder`: The placeholder handle that is defined by Shopify. If you want to set other types of placeholders, check the Shopify documentation.

### Examples

There are many different ways to use this snippet. The easiest one is just by calling it with a render tag.

#### Displaying a placeholder

If the image is blank or doesn't have any image object, it displays a placeholder like the following:

```liquid
{% render 'image' %}
```

You can also pass a placeholder names handle to the snippet:

```liquid
{% render 'image', placeholder: 'product-apparel-4' %}
```

> You can check a list of all the available placeholders illustrations [here](https://shopify.dev/docs/api/liquid/filters#placeholder_svg_tag).

#### Displaying product featured image

You can easily display a product image with the following:

```liquid
{% render 'image' with product.featured_image %}
```

Which is the same as:

```liquid
{% render 'image', image: product.featured_image %}
```

#### Lazyload images in a product grid

You can lazyload images if the grid is made of different product cards, for example, and set the lazyload attribute based on the current position of the image on the grid.

```liquid
{% liquid
  assign lazyload = false
  for product in collection.products
    if forloop.index > 6
      assign lazyload = true
    endif

    render 'image' with product.featured_image, lazyload: lazyload
  endfor
%}
```

#### Set aspect ratio of the image

We recommend using the native browser aspect ratio CSS rule. An example would be to create a class for each of these rules and pass it via the class argument like the following:

```liquid
{% render 'image' with product.featured_image, class: 'aspect-square' %}
```

**If you don't use tailwind, you can set this class on the image instead:**

```css
.image {
  display:block;
  vertical-align:middle
  aspect-ratio: auto;
  height: auto;
  object-fit: cover;
  max-width:100%;
}
```

And then set this on the snippet class default value:

```liquid
  ...
  assign class = 'image ' | append: class
  ...
```

### Running the development server

### Clone the repository

To get started clone the template by clicking the **Use this template** button or by running the following command:

```bash
git clone https://github.com/odestry/theme-image.git
```

### Requirements

To run the development server, you'll need to have [Shopify CLI](#shopify-cli) as well as [Node.js](https://nodejs.org) installed on your machine.

1. Install the dependencies

```bash
npm install
```

2. Connect to your store

To connect your store update the `shopify.theme.toml` file with your store's information.

```toml
[environments.development]
store = "john-apparel.myshopify.com"
```

3. Start the development server

```bash
npm run dev
```

After authenticating, this will start a local development server running at `https://localhost:9292` that you can use to preview your changes.

## Tools

There are a number of really useful tools that the Shopify Themes team uses during development. This theme is already set up to work with these tools.

### Shopify CLI

[Shopify CLI](https://github.com/Shopify/shopify-cli) helps you build Shopify themes faster and is used to automate and enhance your local development workflow. It comes bundled with a suite of commands for developing Shopify themes—everything from working with themes on a Shopify store (e.g. creating, publishing, deleting themes) or launching a development server for local theme development.

You can follow this [quick start guide for theme developers](https://github.com/Shopify/shopify-cli#quick-start-guide-for-theme-developers) to get started.

### Tailwind CSS

[Tailwind CSS](https://tailwindcss.com) is a utility-first CSS framework for rapidly building custom storefront interfaces. It's a great way to build Shopify themes and sections quickly. You can find the configuration file at `tailwind.config.ts`. We use Vite to compile Tailwind CSS.

### Theme Check

We recommend using [Theme Check](https://github.com/shopify/theme-check) as a way to validate and lint your Shopify themes.

Theme Check is a command line tool that runs a series of tests against your theme code to surface errors, deprecations, and potential bugs. It's a great way to ensure your theme is up to date with the latest best practices and that you're not using any deprecated Liquid or JSON fields.

You can also run it from a terminal with the following Shopify CLI command:

```bash
shopify theme check
```

### Continuous Integration

This theme uses [GitHub Actions](https://github.com/features/actions) to maintain the quality of the theme. [This is a starting point](https://github.com/odestry/theme-image/blob/main/.github/workflows/ci.yml) and what we suggest to use in order to ensure you're building better themes. Feel free to build off of it!

#### Shopify/lighthouse-ci-action

We love fast websites! Which is why we created [Shopify/lighthouse-ci-action](https://github.com/Shopify/lighthouse-ci-action). This runs a series of [Google Lighthouse](https://developers.google.com/web/tools/lighthouse) audits for the home, product and collections pages on a store to ensure code that gets added doesn't degrade storefront performance over time.

#### Shopify/theme-check-action

This theme runs [Theme Check](https://github.com/Shopify/theme-check-action) on every commit via [Shopify/theme-check-action](https://github.com/Shopify/theme-check-action).

## Contributing

We'd love your help! Please read our [contributing guide](https://github.com/odestry/.github/blob/main/CONTRIBUTING.md) to learn about our development process, how to propose bug fixes and improvements.

## License

Copyright (c) 2023-present Odestry. See [LICENSE](/LICENSE.md) for further details.
