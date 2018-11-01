# rx-cache

> cache service for javascript

[![NPM](https://img.shields.io/npm/v/rx-cache.svg)](https://www.npmjs.com/package/rx-cache) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## Install

```bash
npm install --save rx-cache
```

## Example using rx-cache

```js
var request = require("request");
const { Observable } = require("rxjs");
const { dispatch, buildPartnerRequest } = require("rx-cache");
const Background = {
  blue: "\x1b[44m%s\x1b[0m",
  yellow: "\x1b[33m%s\x1b[0m",
  red: "\x1b[41m"
};

const partnerRequest = (partner, params, notify) => {
  request.get(
    partner,
    {
      headers: {
        accept: "application/vnd.github.v3+json",
        "user-agent": "not v1.2.3" // v1.2.3 will be current version
      },
      params
    },
    (error, response, body) => {
      const { id, node_id: nodeId } = JSON.parse(body);

      notify({
        id,
        nodeId
      });
    }
  );
};

const partner1 = buildPartnerRequest(
  partnerRequest,
  "https://api.github.com/repos/ysfmag/advanced-react",
  {}
);

const partner2 = buildPartnerRequest(
  partnerRequest,
  "https://api.github.com/repos/ysfmag/amplify-js",
  {}
);

var partner3 = buildPartnerRequest(
  partnerRequest,
  "https://api.github.com/repos/ysfmag/rx-smart-cache",
  {}
);

dispatch(partner3).subscribe(e => {
  console.log("parnter3");
  console.log(Background.blue, JSON.stringify(e));
});

dispatch(partner1).subscribe(e => {
  console.log("parnter1");
  console.log(Background.blue, JSON.stringify(e));
});

dispatch(partner1).then(e => {
  console.log("parnter1  1 then function");
  console.log(Background.red, JSON.stringify(e));
});

dispatch(partner1).then(e => {
  console.log("parnter1  2 then function");
  console.log(Background.red, JSON.stringify(e));
});

dispatch(partner1).then(e => {
  console.log("parnter1 3 then function");
  console.log(Background.red, JSON.stringify(e));
});

dispatch(partner2).subscribe(e => {
  console.log(Background.blue, JSON.stringify(e));
});
```

## License

MIT © [youssef maghzaz](https://github.com/ysfmag)
