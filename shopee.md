const readline = require("readline-sync");

/*
==============================
CATÁLOGO DE PRODUTOS
==============================
*/

const products = [
  { id: 1, name: "Fone Bluetooth", price: 129.9 },
  { id: 2, name: "Mouse Gamer RGB", price: 79.9 },
  { id: 3, name: "Teclado Mecânico", price: 299.9 },
  { id: 4, name: "Monitor 24 Polegadas", price: 899.9 },
  { id: 5, name: "Headset Gamer", price: 199.9 },
  { id: 6, name: "Webcam HD", price: 149.9 },
];

/*
==============================
CARRINHO
==============================
*/

class Cart {
  constructor() {
    this.items = [];
  }

  addProduct(product, quantity) {
    const existing = this.items.find(
      (item) => item.product.id === product.id
    );

    if (existing) {
      existing.quantity += quantity;
    } else {
      this.items.push({
        product,
        quantity,
      });
    }

    console.log("✅ Produto adicionado ao carrinho\n");
  }

  removeProduct(productId) {
    const index = this.items.findIndex(
      (item) => item.product.id === productId
    );

    if (index === -1) {
      console.log("❌ Produto não está no carrinho\n");
      return;
    }

    this.items.splice(index, 1);
    console.log("🗑 Produto removido\n");
  }

  updateQuantity(productId, quantity) {
    const item = this.items.find(
      (item) => item.product.id === productId
    );

    if (!item) {
      console.log("❌ Produto não encontrado no carrinho\n");
      return;
    }

    if (quantity <= 0) {
      this.removeProduct(productId);
      return;
    }

    item.quantity = quantity;

    console.log("🔄 Quantidade atualizada\n");
  }

  getTotalItems() {
    return this.items.reduce((total, item) => total + item.quantity, 0);
  }

  getTotalPrice() {
    return this.items.reduce(
      (total, item) => total + item.product.price * item.quantity,
      0
    );
  }

  showCart() {
    if (this.items.length === 0) {
      console.log("\n🛒 Carrinho vazio\n");
      return;
    }

    console.log("\n======= SEU CARRINHO =======");

    this.items.forEach((item) => {
      const subtotal = item.product.price * item.quantity;

      console.log(
        `${item.product.id} - ${item.product.name}`
      );
      console.log(`Preço: R$ ${item.product.price}`);
      console.log(`Quantidade: ${item.quantity}`);
      console.log(`Subtotal: R$ ${subtotal.toFixed(2)}`);
      console.log("----------------------------");
    });

    console.log("Total de itens:", this.getTotalItems());
    console.log("💰 Total: R$", this.getTotalPrice().toFixed(2));
    console.log("============================\n");
  }
}

const cart = new Cart();

/*
==============================
FUNÇÕES
==============================
*/

function showMenu() {
  console.log("========== SHOPEE CLI ==========");
  console.log("1 - Listar produtos");
  console.log("2 - Adicionar produto ao carrinho");
  console.log("3 - Remover produto do carrinho");
  console.log("4 - Alterar quantidade");
  console.log("5 - Ver carrinho");
  console.log("6 - Finalizar compra");
  console.log("================================");
}

function listProducts() {
  console.log("\n📦 PRODUTOS DISPONÍVEIS:");

  products.forEach((product) => {
    console.log(
      `${product.id} - ${product.name} | R$ ${product.price}`
    );
  });

  console.log();
}

function addProduct() {
  listProducts();

  const id = readline.questionInt("Digite o ID do produto: ");
  const quantity = readline.questionInt("Quantidade: ");

  const product = products.find((p) => p.id === id);

  if (!product) {
    console.log("❌ Produto não encontrado\n");
    return;
  }

  cart.addProduct(product, quantity);
}

function removeProduct() {
  const id = readline.questionInt(
    "Digite o ID do produto para remover: "
  );

  cart.removeProduct(id);
}

function updateQuantity() {
  const id = readline.questionInt("ID do produto: ");
  const quantity = readline.questionInt("Nova quantidade: ");

  cart.updateQuantity(id, quantity);
}

/*
==============================
APP PRINCIPAL
==============================
*/

function main() {
  let running = true;

  while (running) {
    showMenu();

    const option = readline.questionInt("Escolha uma opção: ");

    switch (option) {
      case 1:
        listProducts();
        break;

      case 2:
        addProduct();
        break;

      case 3:
        removeProduct();
        break;

      case 4:
        updateQuantity();
        break;

      case 5:
        cart.showCart();
        break;

      case 6:
        console.log("\n🧾 COMPRA FINALIZADA");
        cart.showCart();
        running = false;
        break;

      default:
        console.log("❌ Opção inválida\n");
    }
  }
}

main();
