# Objective C to Swift Conversion

_A guide for converting projects from Objective-C to Swift_

> [Go to cookbook](https://jlyonsmith.github.io/obj-c-to-swift-conversion/)

## Contributing

If you notice something to improve, the easiest way to do that is to:

1. Fork this repo
2. Set up a branch
3. Make the changes (see `/content`)
4. Submit a pull request

It's just a regular GitHub repository!

Alternatively you can [open an issue](https://github.com/jlyonsmith/objc-c-to-swift-conversion/issues/new) and I'll look into it.

Note that the `gh-pages` branch and wiki content gets generated based on the main repository content.

## Gitbook Generator

The generator converts the wiki content to Gitbook (standalone site). In this case it is pushed to `gh-pages`. Use it as follows:

1. `npm install`
2. `npm run generate-gitbook`

This should generate `/gh-pages`. You can serve that directory through some static server (ie. hit `serve` at `/gh-pages`).

It is possible to deploy the book by hitting `npm run deploy-gitbook`. This will update `gh-pages` branch.

## Credits

The cool GitBook generation technology that this site uses I originally came across in the [React/Webpack Cookbook] (https://github.com/christianalfoni/react-webpack-cookbook)
