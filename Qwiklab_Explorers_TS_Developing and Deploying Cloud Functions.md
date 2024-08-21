# [Developing and Deploying Cloud Functions](https://www.cloudskillsboost.google/course_templates/505/labs/361038)

# # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN) 👍😄🤝

### Run the following Commands in CloudShell

```
export REGION=
```
```
PROJECT_ID=$(gcloud config get-value project)

gcloud services enable \
  artifactregistry.googleapis.com \
  cloudfunctions.googleapis.com \
  cloudbuild.googleapis.com \
  eventarc.googleapis.com \
  run.googleapis.com \
  logging.googleapis.com \
  storage.googleapis.com \
  pubsub.googleapis.com

sleep 60

mkdir ~/btecky && cd $_

cat > index.js <<EOF_END
const functions = require('@google-cloud/functions-framework');

functions.http('convertTemp', (req, res) => {
  var dirn = req.query.convert;
  var ctemp = (req.query.temp - 32) * 5/9;
  var target_unit = 'Celsius';

  if (req.query.temp === undefined) {
    res.status(400);
    res.send('Temperature value not supplied in request.');
  }

  if (dirn === undefined)
    dirn = process.env.TEMP_CONVERT_TO;

  if (dirn === 'ctof') {
    ctemp = (req.query.temp * 9/5) + 32;
    target_unit = 'Fahrenheit';
  }

  res.send("Temperature in " + target_unit + " is: " + ctemp.toFixed(2) + ".");
});

EOF_END

cat > package.json <<EOF_END
{
  "dependencies": {
    "@google-cloud/functions-framework": "^3.0.0"
  }
}
EOF_END

# Function to check if the function exists
function function_exists() {
  gcloud functions describe temperature-converter --region $REGION --format="value(state)" > /dev/null 2>&1
}

# Function to check if the function is active
function check_function_active() {
  gcloud functions describe temperature-converter --region $REGION --format="value(state)" | grep "ACTIVE" > /dev/null
}

# Function to check if the function failed
function check_function_failed() {
  gcloud functions describe temperature-converter --region $REGION --format="value(state)" | grep "FAILED" > /dev/null
}

# Function to deploy the function
function deploy_function() {
  gcloud functions deploy temperature-converter \
    --gen2 \
    --runtime nodejs20 \
    --entry-point convertTemp \
    --source . \
   var ctemp = (temp - 32) * 5 / 9;
   var target_unit = 'Celsius';
 
   if (dirn === undefined) {
     dirn = process.env.TEMP_CONVERT_TO;
   }
 
   if (dirn === 'ctof') {
     ctemp = (temp * 9 / 5) + 32;
     target_unit = 'Fahrenheit';
   }
 
   res.send(\`Temperature in \${target_unit} is: \${ctemp.toFixed(2)}.\`);
 });
 EOF
 
 
 
cat > package.json <<EOF_END
 {
    "name": "temperature-converter",
    "version": "0.0.1",
    "main": "index.js",
    "scripts": {
      "unit-test": "mocha tests/unit*test.js --timeout=6000 --exit",
      "test": "npm -- run unit-test"
    },
    "devDependencies": {
      "mocha": "^9.0.0",
      "sinon": "^14.0.0"
    },
    "dependencies": {
      "@google-cloud/functions-framework": "^2.1.0"
    }
  }

EOF_END
 

mkdir tests && touch tests/unit.http.test.js


cat > tests/unit.http.test.js <<EOF
const {getFunction} = require('@google-cloud/functions-framework/testing');

describe('functions_convert_temperature_http', () => {
  // Sinon is a testing framework that is used to create mocks for Node.js applications written in Express.
  // Express is Node.js web application framework used to implement HTTP functions.
  const sinon = require('sinon');
  const assert = require('assert');
  require('../');

  const getMocks = () => {
    const req = {body: {}, query: {}};

    return {
      req: req,
      res: {
        send: sinon.stub().returnsThis(),
        status: sinon.stub().returnsThis()
      },
    };
  };

  let envOrig;
  before(() => {
    envOrig = JSON.stringify(process.env);
  });

  after(() => {
    process.env = JSON.parse(envOrig);
  });

  it('convertTemp: should convert a Fahrenheit temp value by default', () => {
    const mocks = getMocks();
    mocks.req.query = {temp: 70};

    const convertTemp = getFunction('convertTemp');
    convertTemp(mocks.req, mocks.res);
    assert.strictEqual(mocks.res.send.calledOnceWith('Temperature in Celsius is: 21.11.'), true);
  });

  it('convertTemp: should convert a Celsius temp value', () => {
    const mocks = getMocks();
    mocks.req.query = {temp: 21.11, convert: 'ctof'};

    const convertTemp = getFunction('convertTemp');
    convertTemp(mocks.req, mocks.res);
    assert.strictEqual(mocks.res.send.calledOnceWith('Temperature in Fahrenheit is: 70.00.'), true);
  });

  it('convertTemp: should convert a Celsius temp value by default', () => {
    process.env.TEMP_CONVERT_TO = 'ctof';
    const mocks = getMocks();
    mocks.req.query = {temp: 21.11};

    const convertTemp = getFunction('convertTemp');
    convertTemp(mocks.req, mocks.res);
    assert.strictEqual(mocks.res.send.calledOnceWith('Temperature in Fahrenheit is: 70.00.'), true);
  });

  it('convertTemp: should return an error message', () => {
    const mocks = getMocks();

    const convertTemp = getFunction('convertTemp');
    convertTemp(mocks.req, mocks.res);

    assert.strictEqual(mocks.res.status.calledOnce, true);
    assert.strictEqual(mocks.res.status.firstCall.args[0], 400);
  });
});
EOF



npm install

npm test


gcloud run deploy temperature-converter \
--image=$REGION-docker.pkg.dev/$PROJECT_ID/gcf-artifacts/temperature--converter:version_1 \
--set-env-vars=TEMP_CONVERT_TO=ctof \
--region=$REGION \
--project=$PROJECT_ID \
 && gcloud run services update-traffic temperature-converter --to-latest --region=$REGION
```

# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)
