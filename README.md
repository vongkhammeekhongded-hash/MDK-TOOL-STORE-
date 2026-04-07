import React, { useState, useEffect, ChangeEvent } from 'react';

interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  image: string;
}

export default function MDKToolStore() {
  const [products, setProducts] = useState<Product[]>(() => {
    const saved = localStorage.getItem('mdk-products');
    return saved
      ? JSON.parse(saved)
      : [
          { id: 1, name: 'ທໍ່ HDPE', price: 50000, category: 'ທໍ່', image: '/pipe.jpg' },
          { id: 2, name: 'ສີພ້ນ', price: 44000, category: 'ສີ', image: '/spray.jpg' }
        ];
  });

  const [cart, setCart] = useState<Product[]>(() => {
    const saved = localStorage.getItem('mdk-cart');
    return saved ? JSON.parse(saved) : [];
  });

  const [newName, setNewName] = useState<string>('');
  const [newPrice, setNewPrice] = useState<string>('');

  useEffect(() => {
    localStorage.setItem('mdk-products', JSON.stringify(products));
  }, [products]);

  useEffect(() => {
    localStorage.setItem('mdk-cart', JSON.stringify(cart));
  }, [cart]);

  const addToCart = (product: Product) => setCart([...cart, product]);

  const addProduct = () => {
    if (!newName || !newPrice) return;
    setProducts([
      ...products,
      { id: Date.now(), name: newName, price: Number(newPrice), category: 'ທົ່ວໄປ', image: '/pipe.jpg' }
    ]);
    setNewName('');
    setNewPrice('');
  };

  const handleNameChange = (e: ChangeEvent<HTMLInputElement>) => setNewName(e.target.value);
  const handlePriceChange = (e: ChangeEvent<HTMLInputElement>) => setNewPrice(e.target.value);

  return (
    <div className="min-h-screen bg-black text-white p-6">
      <h1 className="text-4xl font-bold text-orange-500 mb-6">MDK TOOL STORE 🛠️</h1>

      <div className="grid md:grid-cols-3 gap-4 mb-8">
        <input value={newName} onChange={handleNameChange} placeholder="ຊື່ສິນຄ້າ" className="p-2 rounded text-black" />
        <input value={newPrice} onChange={handlePriceChange} placeholder="ລາຄາ" className="p-2 rounded text-black" />
        <button onClick={addProduct} className="bg-orange-500 rounded p-2">ເພີ່ມສິນຄ້າ</button>
      </div>

      <div className="grid md:grid-cols-3 gap-6">
        {products.map((p) => (
          <div key={p.id} className="bg-zinc-900 p-4 rounded-2xl shadow">
            <img src={p.image} alt={p.name} className="rounded mb-3" />
            <h2 className="text-xl font-bold">{p.name}</h2>
            <p>{p.price.toLocaleString()} ກີບ</p>
            <button onClick={() => addToCart(p)} className="mt-3 bg-orange-500 px-4 py-2 rounded">ໃສ່ກະຕ່າ</button>
          </div>
        ))}
      </div>

      <section className="mt-10 bg-zinc-900 p-6 rounded-2xl">
        <h2 className="text-2xl font-bold mb-4">ກະຕ່າສິນຄ້າ ({cart.length})</h2>
        {cart.map((item, i) => (
          <p key={i}>{item.name} - {item.price.toLocaleString()} ກີບ</p>
        ))}
      </section>

      <section className="mt-10 bg-zinc-900 p-6 rounded-2xl text-center">
        <h2 className="text-2xl font-bold mb-4">QR ຈ່າຍເງິນ</h2>
        <img src="/qr-payment.png" alt="QR Payment" className="mx-auto rounded-xl mb-4 max-w-xs" />
        <p className="text-lg font-bold">02098832516</p>
      </section>
    </div>
  );
}
