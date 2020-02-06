# Tailwind and PostCSS Starter Template

Setup Tailwind with PurgeCSS and PostCSS starter Template

## Install Tailwind

        npm init -y
        npm install tailwindcss

## Create the configuration file

        npx tailwind init

## Configure PostCSS

In your root folder create a file named 'postcss.config.js' and run this command to install autoprefixer and postcss-cli

        npm install autoprefixer
        npm install -g postcss-cli

and paste this in your file 'postcss.config.js'

        module.exports = {
        plugins: [
            require('tailwindcss'),
            require('autoprefixer')
        ]
        }

## Create the Tailwind CSS file

create folder named 'src' and file named 'tailwind.css' paste this on it

        @tailwind base;
        @tailwind components;
        @tailwind utilities;

## Create the build command

Now open your package.json file, and add a scripts section if you don’t have it:

        "scripts": {
        "build:css": "postcss src/css/tailwind.css -o public/build/main.css"
        }

## Build Tailwind

Now from the command line run command below will build the final CSS file.

        npm run build:css

The resulting file is in /public/dist/main.css

<!-- ## Automatically regenerate the CSS upon file changes

simply run this command

        npm install watch

and add the watch script to your package.json file

        "scripts": {
        "build:css": "postcss src/tailwind.css -o static/dist/tailwind.css",
        "watch": "watch 'npm run build:css' ./layouts"
        }

Now run 

        npm run watch

and you should be good to go! -->

## Trim the file size

        npm install cssnano
        npm install @fullhuman/postcss-purgecss

Then we add this to our PostCSS configuration file postcss.config.js:
In development, avoid too much processing

        const purgecss = require('@fullhuman/postcss-purgecss')
        const cssnano = require('cssnano')

        module.exports = {
        plugins: [
            require('tailwindcss'),
            process.env.NODE_ENV === 'production' ? require('autoprefixer') : null,
            process.env.NODE_ENV === 'production'
            ? cssnano({ preset: 'default' })
            : null,
            purgecss({
            content: ['./public/**/*.html', './src/**/*.vue', './src/**/*.jsx'],
            defaultExtractor: content => content.match(/[\w-/:]+(?<!:)/g) || []
            })
        ]
        }

