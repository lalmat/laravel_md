# Enable Hot Module Reloading (aka `sail npm run hot`)under Laravel Sail

## Enable

sail down
nano docker-compose.yml

Line 13
	ports :
	    - '${APP_PORT:-80}:80'
	    **- '${APP_PORT_HOT:-8080}:8080'**

nano webpack.mix.js

if (!mix.inProduction()) {
  mix.webpackConfig({
    devServer: {
        host: '0.0.0.0',
        port: 8080,
    },
  });
}

sail up

use APP_PORT_HOT in .env to customize