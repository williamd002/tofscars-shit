# spotify-app

## deployed on
https://collabqueue.com

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### TOFS Usecase
This package requires a redis server. Docker can be used locally for this wusing the following command:
```
docker-compose up -d
```

`pm2` package is suggested to keep running `node src/server/www.js`.