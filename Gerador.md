import QRCode from "qrcode";
import readline from "readline";
import fs from "fs";

if (!fs.existsSync("qrcodes")) fs.mkdirSync("qrcodes");

const rl = readline.createInterface({ input: process.stdin, output: process.stdout });

rl.question("Nome do produto: ", productName => {
  rl.question("Link do produto: ", async productUrl => {
    const filePath = `qrcodes/${productName.replace(/\s+/g,"_").toLowerCase()}.png`;
    try { await QRCode.toFile(filePath, productUrl);
      console.log(`\n✅ QR Code gerado: ${filePath}`); 
    } catch(e){ console.log("Erro:", e); }
    rl.close();
  });
});
