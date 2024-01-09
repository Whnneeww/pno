const fs = require('fs');
const https = require('https');
const url = require('url');
const querystring = require('querystring');

function downloadFile(urlString) {
  return new Promise((resolve, reject) => {
    const parsedUrl = url.parse(urlString);
    const options = {
      hostname: parsedUrl.hostname,
      path: parsedUrl.path,
      method: 'GET',
    };
    const req = https.request(options, (response) => {
      let data = Buffer.from([]);
      response.on('data', (chunk) => {
        data = Buffer.concat([data, chunk]);
      });
      response.on('end', () => {
        resolve(data);
      });
      response.on('error', (error) => {
        reject(error);
      });
    });
    req.on('error', (error) => {
      reject(error);
    });
    req.end();
  });
}

function convertToHex(binaryData) {
  return binaryData.toString('hex');
}

function saveAsTxt(hexData, filename) {
  fs.writeFileSync(filename, hexData);
}

function displayTxtFile(filename) {
  const fileContent = fs.readFileSync(filename, 'utf8');
  console.log(fileContent);
}

async function main(urlString, filename) {
  try {
    const binaryData = await downloadFile(urlString);
    const hexData = convertToHex(binaryData);
    saveAsTxt(hexData, filename);
    displayTxtFile(filename);
  } catch (error) {
    console.error(error);
  }
}

const urlString = process.argv[2]; // コマンドライン引数からURLを取得
const parsedUrl = url.parse(urlString);
const queryParameters = querystring.parse(parsedUrl.query);
const filename = queryParameters.filename || 'output.txt'; // 保存するファイル名

main(urlString, filename);
