const express = require('express');
const https = require('https');
const fs = require('fs');
const rtspRelay = require('rtsp-relay');
const cors = require('cors')
const app = express();
app.use(cors());

const options = {
  key: fs.readFileSync("./__vps_vn.key", 'utf8'),
  cert: fs.readFileSync("./star_vps_vn_cert.pem", 'utf8'),
};

const server = https.createServer(options, app);

const { proxy, scriptUrl } = rtspRelay(app, server);

//Get 
app.ws('/api/stream/:username/:password/:domain/:channel/:port', (ws, req) => {
  proxy({
    url: `rtsp://${req.params.username}:${req.params.password}@${req.params.domain}.kbvision.tv:${req.params.port}/cam/realmonitor?channel=${req.params.channel}&subtype=0`,
    verbose: false,
    transport: 'tcp'
  })(ws)
}
);

// this is an example html page to view the stream
app.get('/:username/:password/:domain/:channel', (req, res) => {
  if (!req.params.domain.split(':')[1]) {
    res.send('<h1>Params Error</h1>')
    return;
  }
  let domain = req.params.domain.split(':')[0];
  let port = req.params.domain.split(':')[1];
  console.log('Camera is running!')
  res.send(`
  <canvas id='canvas' width='640' height='480'></canvas>
  <script src='${scriptUrl}'></script>
  <script>
    loadPlayer({
      url: 'wss://' + location.host + '/api/stream/${req.params.username}/${req.params.password}/${domain}/${req.params.channel}/${port}',
      canvas: document.getElementById('canvas')
    });
    
  </script>
`)
}
);

app.get('/', (req, res) => {
  res.send('<h1>Hello World!</h1>')
})

const PORT = 8000;
server.listen(PORT, () => {
  console.log('Server is running port ' + PORT)
});