# Enable Hot Module Reloading with Laravel Sail

> theses lines enable the command `sail npm run hot` which allows hot module reloading

## 1. Stop sail if running
```bash
my-project$ sail down
```

## 2. Add hot reload port
Edit `[YOUR_PROJECT_ROOT]/docker-compose.yml` and look at line 13 to add the "APP_PORT_HOT" line below
```yml
  ...
	ports :
	    - '${APP_PORT:-80}:80'
	    - '${APP_PORT_HOT:-8080}:8080'
  ...
```
> Note : You can then use APP_PORT_HOT in your .env file to customize the port

## 3. Enable hot module reloading
Edit `[YOUR_PROJECT_ROOT]/webpack.mix.js` and add this block at the end of the file
```js
if (!mix.inProduction()) {
  mix.webpackConfig({
    devServer: {
        host: '0.0.0.0',
        port: 8080,
    },
  });
}
```
> Note : The port number specified here must match with the APP_PORT_HOT if defined

## 4. Restart sail & hot reloading
```
my-project$ sail up -d
my-project$ sail npm run hot
```

Happy reloading !