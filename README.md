import { X, Plus, Minus, Trash2, MessageCircle } from 'lucide-react';
import { useCart } from '@/contexts/CartContext';
import { Button } from '@/components/ui/button';
import { Sheet, SheetContent, SheetHeader, SheetTitle } from '@/components/ui/sheet';

const CartDrawer = () => {
  const { items, isCartOpen, setIsCartOpen, removeFromCart, updateQuantity, totalPrice, clearCart } = useCart();

  const generateWhatsAppMessage = () => {
    let message = '🛒 *Pedido URBAN*\n\n';
    items.forEach(item => {
      message += `• ${item.product.name}\n`;
      message += `  Tamanho: ${item.size} | Cor: ${item.color}\n`;
      message += `  Qtd: ${item.quantity} x R$ ${item.product.price.toFixed(2).replace('.', ',')}\n\n`;
    });
    message += `\n*Total: R$ ${totalPrice.toFixed(2).replace('.', ',')}*`;
    return encodeURIComponent(message);
  };

  const handleCheckout = () => {
    const phoneNumber = '5511999999999';
    const message = generateWhatsAppMessage();
    window.open(`https://wa.me/${phoneNumber}?text=${message}`, '_blank');
  };

  return (
    <Sheet open={isCartOpen} onOpenChange={setIsCartOpen}>
      <SheetContent className="w-full sm:max-w-lg bg-background border-l border-border">
        <SheetHeader className="border-b border-border pb-4">
          <SheetTitle className="text-xl font-bold">Seu Carrinho</SheetTitle>
        </SheetHeader>

        {items.length === 0 ? (
          <div className="flex flex-col items-center justify-center h-[60vh]">
            <div className="w-20 h-20 bg-secondary rounded-full flex items-center justify-center mb-4">
              <svg className="w-10 h-10 text-muted-foreground" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={1.5} d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
              </svg>
            </div>
            <p className="text-muted-foreground text-center">
              Seu carrinho está vazio.<br />
              Adicione alguns produtos!
            </p>
          </div>
        ) : (
          <>
            <div className="flex-1 overflow-y-auto py-4 space-y-4 max-h-[50vh]">
              {items.map(item => (
                <div
                  key={`${item.product.id}-${item.size}-${item.color}`}
                  className="flex gap-4 p-3 bg-secondary rounded-lg"
                >
                  <img
                    src={item.product.image}
                    alt={item.product.name}
                    className="w-20 h-20 object-cover rounded"
                  />
                  <div className="flex-1 min-w-0">
                    <h4 className="font-semibold text-sm truncate">{item.product.name}</h4>
                    <p className="text-xs text-muted-foreground">
                      {item.size} • {item.color}
                    </p>
                    <p className="text-primary font-bold mt-1">
                      R$ {item.product.price.toFixed(2).replace('.', ',')}
                    </p>
                    <div className="flex items-center gap-2 mt-2">
                      <button
                        onClick={() => updateQuantity(item.product.id, item.size, item.color, item.quantity - 1)}
                        className="w-7 h-7 bg-card rounded flex items-center justify-center hover:bg-primary hover:text-primary-foreground transition-colors"
                      >
                        <Minus className="w-4 h-4" />
                      </button>
                      <span className="w-8 text-center font-medium">{item.quantity}</span>
                      <button
                        onClick={() => updateQuantity(item.product.id, item.size, item.color, item.quantity + 1)}
                        className="w-7 h-7 bg-card rounded flex items-center justify-center hover:bg-primary hover:text-primary-foreground transition-colors"
                      >
                        <Plus className="w-4 h-4" />
                      </button>
                      <button
                        onClick={() => removeFromCart(item.product.id, item.size, item.color)}
                        className="ml-auto p-1 text-muted-foreground hover:text-destructive transition-colors"
                      >
                        <Trash2 className="w-4 h-4" />
                      </button>
                    </div>
                  </div>
                </div>
              ))}
            </div>

            <div className="border-t border-border pt-4 mt-4 space-y-4">
              <div className="flex items-center justify-between text-lg">
                <span className="font-medium">Total:</span>
                <span className="font-bold text-primary">
                  R$ {totalPrice.toFixed(2).replace('.', ',')}
                </span>
              </div>

              <Button
                variant="whatsapp"
                size="xl"
                className="w-full"
                onClick={handleCheckout}
              >
                <MessageCircle className="w-5 h-5 mr-2" />
                Finalizar no WhatsApp
              </Button>

              <button
                onClick={clearCart}
                className="w-full text-sm text-muted-foreground hover:text-destructive transition-colors"
              >
                Limpar carrinho
              </button>
            </div>
          </>
        )}
      </SheetContent>
    </Sheet>
  );
};

export default CartDrawer;
import { Link } from 'react-router-dom';
import { Instagram, MessageCircle } from 'lucide-react';

const Footer = () => {
  const currentYear = new Date().getFullYear();

  return (
    <footer className="bg-secondary border-t border-border">
      <div className="container-main py-12 md:py-16">
        <div className="grid grid-cols-1 md:grid-cols-4 gap-8 md:gap-12">
          {/* Brand */}
          <div className="md:col-span-1">
            <Link to="/" className="inline-block mb-4">
              <span className="text-2xl font-black tracking-tighter">
                URBAN<span className="text-primary">.</span>
              </span>
            </Link>
            <p className="text-muted-foreground text-sm leading-relaxed">
              Moda urbana autêntica para quem vive o street style. Vista sua atitude.
            </p>
          </div>

          {/* Links */}
          <div>
            <h4 className="font-bold uppercase tracking-wider mb-4 text-sm">Navegação</h4>
            <ul className="space-y-2">
              {[
                { href: '/', label: 'Home' },
                { href: '/catalogo', label: 'Catálogo' },
                { href: '/sobre', label: 'Sobre' },
                { href: '/contato', label: 'Contato' },
              ].map(link => (
                <li key={link.href}>
                  <Link
                    to={link.href}
                    className="text-muted-foreground hover:text-primary transition-colors text-sm"
                  >
                    {link.label}
                  </Link>
                </li>
              ))}
            </ul>
          </div>

          {/* Categories */}
          <div>
            <h4 className="font-bold uppercase tracking-wider mb-4 text-sm">Categorias</h4>
            <ul className="space-y-2">
              {['Camisetas', 'Calças', 'Moletons', 'Acessórios'].map(cat => (
                <li key={cat}>
                  <Link
                    to="/catalogo"
                    className="text-muted-foreground hover:text-primary transition-colors text-sm"
                  >
                    {cat}
                  </Link>
                </li>
              ))}
            </ul>
          </div>

          {/* Social */}
          <div>
            <h4 className="font-bold uppercase tracking-wider mb-4 text-sm">Redes Sociais</h4>
            <div className="flex gap-4">
              <a
                href="https://instagram.com"
                target="_blank"
                rel="noopener noreferrer"
                className="w-10 h-10 bg-card rounded-full flex items-center justify-center hover:bg-primary hover:text-primary-foreground transition-colors"
                aria-label="Instagram"
              >
                <Instagram className="w-5 h-5" />
              </a>
              <a
                href="https://tiktok.com"
                target="_blank"
                rel="noopener noreferrer"
                className="w-10 h-10 bg-card rounded-full flex items-center justify-center hover:bg-primary hover:text-primary-foreground transition-colors"
                aria-label="TikTok"
              >
                <svg className="w-5 h-5" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z" />
                </svg>
              </a>
              <a
                href="https://wa.me/5511999999999"
                target="_blank"
                rel="noopener noreferrer"
                className="w-10 h-10 bg-card rounded-full flex items-center justify-center hover:bg-[#25D366] hover:text-foreground transition-colors"
                aria-label="WhatsApp"
              >
                <MessageCircle className="w-5 h-5" />
              </a>
            </div>
          </div>
        </div>

        <div className="mt-12 pt-8 border-t border-border flex flex-col md:flex-row items-center justify-between gap-4">
          <p className="text-muted-foreground text-sm">
            © {currentYear} URBAN. Todos os direitos reservados.
          </p>
          <p className="text-muted-foreground text-sm">
            Feito com 💛 no Brasil
          </p>
        </div>
      </div>
    </footer>
  );
};

export default Footer;
import { Link, useLocation } from 'react-router-dom';
import { ShoppingBag, Menu, X } from 'lucide-react';
import { useState } from 'react';
import { useCart } from '@/contexts/CartContext';
import { Button } from '@/components/ui/button';

const Header = () => {
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const { totalItems, setIsCartOpen } = useCart();
  const location = useLocation();

  const navLinks = [
    { href: '/', label: 'Home' },
    { href: '/catalogo', label: 'Catálogo' },
    { href: '/sobre', label: 'Sobre' },
    { href: '/contato', label: 'Contato' },
  ];

  const isActive = (path: string) => location.pathname === path;

  return (
    <header className="fixed top-0 left-0 right-0 z-50 bg-background/95 backdrop-blur-md border-b border-border">
      <div className="container-main">
        <div className="flex items-center justify-between h-16 md:h-20">
          {/* Logo */}
          <Link to="/" className="flex items-center gap-2">
            <span className="text-2xl md:text-3xl font-black tracking-tighter">
              URBAN<span className="text-primary">.</span>
            </span>
          </Link>

          {/* Desktop Navigation */}
          <nav className="hidden md:flex items-center gap-8">
            {navLinks.map(link => (
              <Link
                key={link.href}
                to={link.href}
                className={`text-sm font-medium uppercase tracking-wider transition-colors hover:text-primary ${
                  isActive(link.href) ? 'text-primary' : 'text-foreground'
                }`}
              >
                {link.label}
              </Link>
            ))}
          </nav>

          {/* Actions */}
          <div className="flex items-center gap-4">
            <button
              onClick={() => setIsCartOpen(true)}
              className="relative p-2 hover:bg-secondary rounded-full transition-colors"
              aria-label="Abrir carrinho"
            >
              <ShoppingBag className="w-6 h-6" />
              {totalItems > 0 && (
                <span className="absolute -top-1 -right-1 w-5 h-5 bg-primary text-primary-foreground text-xs font-bold rounded-full flex items-center justify-center">
                  {totalItems}
                </span>
              )}
            </button>

            {/* Mobile Menu Button */}
            <Button
              variant="ghost"
              size="icon"
              className="md:hidden"
              onClick={() => setIsMenuOpen(!isMenuOpen)}
              aria-label="Menu"
            >
              {isMenuOpen ? <X className="w-6 h-6" /> : <Menu className="w-6 h-6" />}
            </Button>
          </div>
        </div>

        {/* Mobile Navigation */}
        {isMenuOpen && (
          <nav className="md:hidden py-4 border-t border-border animate-slide-up">
            {navLinks.map(link => (
              <Link
                key={link.href}
                to={link.href}
                onClick={() => setIsMenuOpen(false)}
                className={`block py-3 text-lg font-medium uppercase tracking-wider transition-colors hover:text-primary ${
                  isActive(link.href) ? 'text-primary' : 'text-foreground'
                }`}
              >
                {link.label}
              </Link>
            ))}
          </nav>
        )}
      </div>
    </header>
  );
};

export default Header;
import { Link } from 'react-router-dom';
import { Product } from '@/data/products';
import { Button } from '@/components/ui/button';
import { useCart } from '@/contexts/CartContext';

interface ProductCardProps {
  product: Product;
}

const ProductCard = ({ product }: ProductCardProps) => {
  const { addToCart } = useCart();
  const hasDiscount = product.originalPrice && product.originalPrice > product.price;
  const discountPercent = hasDiscount
    ? Math.round(((product.originalPrice! - product.price) / product.originalPrice!) * 100)
    : 0;

  const handleQuickAdd = (e: React.MouseEvent) => {
    e.preventDefault();
    e.stopPropagation();
    addToCart(product, product.sizes[0], product.colors[0].name);
  };

  return (
    <Link to={`/produto/${product.id}`} className="group block">
      <div className="relative overflow-hidden rounded-lg bg-card card-hover">
        {/* Badges */}
        <div className="absolute top-3 left-3 z-10 flex flex-col gap-2">
          {product.isNew && (
            <span className="px-2 py-1 bg-primary text-primary-foreground text-xs font-bold uppercase">
              Novo
            </span>
          )}
          {hasDiscount && (
            <span className="px-2 py-1 bg-destructive text-destructive-foreground text-xs font-bold uppercase">
              -{discountPercent}%
            </span>
          )}
        </div>

        {/* Image */}
        <div className="aspect-[3/4] overflow-hidden">
          <img
            src={product.image}
            alt={product.name}
            className="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
          />
        </div>

        {/* Quick Add Button - appears on hover */}
        <div className="absolute bottom-0 left-0 right-0 p-4 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
          <Button
            variant="hero"
            className="w-full"
            onClick={handleQuickAdd}
          >
            Comprar
          </Button>
        </div>
      </div>

      {/* Info */}
      <div className="mt-4 space-y-1">
        <h3 className="font-semibold text-foreground group-hover:text-primary transition-colors">
          {product.name}
        </h3>
        <div className="flex items-center gap-2">
          <span className="text-lg font-bold text-primary">
            R$ {product.price.toFixed(2).replace('.', ',')}
          </span>
          {hasDiscount && (
            <span className="text-sm text-muted-foreground line-through">
              R$ {product.originalPrice!.toFixed(2).replace('.', ',')}
            </span>
          )}
        </div>
      </div>
    </Link>
  );
};

export default ProductCard;
import { MessageCircle } from 'lucide-react';

const WhatsAppButton = () => {
  const phoneNumber = '5511999999999';
  const message = encodeURIComponent('Olá! Vim pelo site e gostaria de saber mais sobre os produtos.');

  return (
    <a
      href={`https://wa.me/${phoneNumber}?text=${message}`}
      target="_blank"
      rel="noopener noreferrer"
      className="fixed bottom-6 right-6 z-50 w-14 h-14 bg-[#25D366] rounded-full flex items-center justify-center shadow-lg hover:scale-110 transition-transform animate-float"
      aria-label="Fale conosco pelo WhatsApp"
    >
      <MessageCircle className="w-7 h-7 text-foreground" />
    </a>
  );
};

export default WhatsAppButton;
import React, { createContext, useContext, useState, useEffect, ReactNode } from 'react';
import { Product } from '@/data/products';

export interface CartItem {
  product: Product;
  quantity: number;
  size: string;
  color: string;
}

interface CartContextType {
  items: CartItem[];
  addToCart: (product: Product, size: string, color: string) => void;
  removeFromCart: (productId: string, size: string, color: string) => void;
  updateQuantity: (productId: string, size: string, color: string, quantity: number) => void;
  clearCart: () => void;
  totalItems: number;
  totalPrice: number;
  isCartOpen: boolean;
  setIsCartOpen: (open: boolean) => void;
}

const CartContext = createContext<CartContextType | undefined>(undefined);

export const CartProvider = ({ children }: { children: ReactNode }) => {
  const [items, setItems] = useState<CartItem[]>(() => {
    const saved = localStorage.getItem('cart');
    return saved ? JSON.parse(saved) : [];
  });
  const [isCartOpen, setIsCartOpen] = useState(false);

  useEffect(() => {
    localStorage.setItem('cart', JSON.stringify(items));
  }, [items]);

  const addToCart = (product: Product, size: string, color: string) => {
    setItems(prev => {
      const existing = prev.find(
        item => item.product.id === product.id && item.size === size && item.color === color
      );
      if (existing) {
        return prev.map(item =>
          item.product.id === product.id && item.size === size && item.color === color
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { product, quantity: 1, size, color }];
    });
    setIsCartOpen(true);
  };

  const removeFromCart = (productId: string, size: string, color: string) => {
    setItems(prev =>
      prev.filter(
        item => !(item.product.id === productId && item.size === size && item.color === color)
      )
    );
  };

  const updateQuantity = (productId: string, size: string, color: string, quantity: number) => {
    if (quantity <= 0) {
      removeFromCart(productId, size, color);
      return;
    }
    setItems(prev =>
      prev.map(item =>
        item.product.id === productId && item.size === size && item.color === color
          ? { ...item, quantity }
          : item
      )
    );
  };

  const clearCart = () => setItems([]);

  const totalItems = items.reduce((sum, item) => sum + item.quantity, 0);
  const totalPrice = items.reduce((sum, item) => sum + item.product.price * item.quantity, 0);

  return (
    <CartContext.Provider
      value={{
        items,
        addToCart,
        removeFromCart,
        updateQuantity,
        clearCart,
        totalItems,
        totalPrice,
        isCartOpen,
        setIsCartOpen,
      }}
    >
      {children}
    </CartContext.Provider>
  );
};

export const useCart = () => {
  const context = useContext(CartContext);
  if (!context) {
    throw new Error('useCart must be used within a CartProvider');
  }
  return context;
};
import tshirt1 from '@/assets/products/tshirt-1.jpg';
import pants1 from '@/assets/products/pants-1.jpg';
import hoodie1 from '@/assets/products/hoodie-1.jpg';
import sunglasses1 from '@/assets/products/sunglasses-1.jpg';
import cap1 from '@/assets/products/cap-1.jpg';
import socks1 from '@/assets/products/socks-1.jpg';

export interface Product {
  id: string;
  name: string;
  description: string;
  price: number;
  originalPrice?: number;
  image: string;
  images: string[];
  category: 'camisetas' | 'calcas' | 'shorts' | 'oculos' | 'bones' | 'meias' | 'acessorios';
  sizes: string[];
  colors: { name: string; hex: string }[];
  isNew?: boolean;
  isBestSeller?: boolean;
  isPromotion?: boolean;
}

export const products: Product[] = [
  {
    id: 'camiseta-urban-vibes',
    name: 'Camiseta Urban Vibes',
    description: 'Camiseta oversized com estampa exclusiva. Tecido 100% algodão premium, corte relaxado perfeito para o dia a dia. Visual autêntico que representa a cultura urbana.',
    price: 149.90,
    originalPrice: 189.90,
    image: tshirt1,
    images: [tshirt1],
    category: 'camisetas',
    sizes: ['P', 'M', 'G', 'GG'],
    colors: [
      { name: 'Preto', hex: '#000000' },
      { name: 'Branco', hex: '#FFFFFF' },
    ],
    isNew: true,
    isBestSeller: true,
  },
  {
    id: 'calca-cargo-street',
    name: 'Calça Cargo Street',
    description: 'Calça cargo com bolsos laterais, tecido resistente e confortável. Design funcional com estética street que combina com qualquer ocasião.',
    price: 259.90,
    image: pants1,
    images: [pants1],
    category: 'calcas',
    sizes: ['38', '40', '42', '44', '46'],
    colors: [
      { name: 'Preto', hex: '#000000' },
      { name: 'Caqui', hex: '#8B7355' },
    ],
    isBestSeller: true,
  },
  {
    id: 'moletom-cuakl',
    name: 'Moletom CUAKL Signature',
    description: 'Moletom oversized com capuz, bolso canguru e estampa exclusiva. Tecido flanelado super macio, ideal para os dias mais frios.',
    price: 289.90,
    originalPrice: 349.90,
    image: hoodie1,
    images: [hoodie1],
    category: 'camisetas',
    sizes: ['P', 'M', 'G', 'GG'],
    colors: [
      { name: 'Preto', hex: '#000000' },
    ],
    isNew: true,
    isPromotion: true,
  },
  {
    id: 'oculos-classic-black',
    name: 'Óculos Classic Black',
    description: 'Óculos de sol com armação preta e lentes polarizadas. Proteção UV400 com design atemporal que complementa qualquer visual.',
    price: 199.90,
    image: sunglasses1,
    images: [sunglasses1],
    category: 'oculos',
    sizes: ['Único'],
    colors: [
      { name: 'Preto', hex: '#000000' },
    ],
    isBestSeller: true,
  },
  {
    id: 'bone-snapback',
    name: 'Boné Snapback Original',
    description: 'Boné snapback com aba reta e fechamento ajustável. Bordado de alta qualidade, ideal para completar o look street.',
    price: 119.90,
    image: cap1,
    images: [cap1],
    category: 'bones',
    sizes: ['Único'],
    colors: [
      { name: 'Preto', hex: '#000000' },
    ],
    isNew: true,
  },
  {
    id: 'meias-zigzag',
    name: 'Meias ZigZag Pack',
    description: 'Kit com 3 pares de meias cano alto com estampa exclusiva. Algodão com elastano para maior conforto e durabilidade.',
    price: 59.90,
    originalPrice: 79.90,
    image: socks1,
    images: [socks1],
    category: 'meias',
    sizes: ['37-40', '41-44'],
    colors: [
      { name: 'Preto/Amarelo', hex: '#FFD100' },
    ],
    isPromotion: true,
  },
];

export const categories = [
  { id: 'camisetas', name: 'Camisetas', icon: '👕' },
  { id: 'calcas', name: 'Calças', icon: '👖' },
  { id: 'shorts', name: 'Shorts', icon: '🩳' },
  { id: 'oculos', name: 'Óculos', icon: '🕶️' },
  { id: 'bones', name: 'Bonés', icon: '🧢' },
  { id: 'meias', name: 'Meias', icon: '🧦' },
  { id: 'acessorios', name: 'Acessórios', icon: '⛓️' },
];

export const getProductById = (id: string): Product | undefined => {
  return products.find(product => product.id === id);
};

export const getProductsByCategory = (category: string): Product[] => {
  return products.filter(product => product.category === category);
};

export const getNewProducts = (): Product[] => {
  return products.filter(product => product.isNew);
};

export const getBestSellers = (): Product[] => {
  return products.filter(product => product.isBestSeller);
};

export const getPromotions = (): Product[] => {
  return products.filter(product => product.isPromotion);
};
import { useState, useMemo } from 'react';
import { useSearchParams } from 'react-router-dom';
import { SlidersHorizontal, X } from 'lucide-react';
import ProductCard from '@/components/ProductCard';
import { products, categories } from '@/data/products';
import { Button } from '@/components/ui/button';

const Catalogo = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const [isFilterOpen, setIsFilterOpen] = useState(false);

  const selectedCategory = searchParams.get('categoria') || '';
  const selectedPrice = searchParams.get('preco') || '';
  const selectedSize = searchParams.get('tamanho') || '';

  const priceRanges = [
    { id: '0-100', label: 'Até R$ 100' },
    { id: '100-200', label: 'R$ 100 - R$ 200' },
    { id: '200-300', label: 'R$ 200 - R$ 300' },
    { id: '300+', label: 'Acima de R$ 300' },
  ];

  const sizes = ['P', 'M', 'G', 'GG', '38', '40', '42', '44', '46', 'Único'];

  const filteredProducts = useMemo(() => {
    let result = [...products];

    if (selectedCategory) {
      result = result.filter(p => p.category === selectedCategory);
    }

    if (selectedPrice) {
      result = result.filter(p => {
        const price = p.price;
        switch (selectedPrice) {
          case '0-100': return price <= 100;
          case '100-200': return price > 100 && price <= 200;
          case '200-300': return price > 200 && price <= 300;
          case '300+': return price > 300;
          default: return true;
        }
      });
    }

    if (selectedSize) {
      result = result.filter(p => p.sizes.includes(selectedSize));
    }

    return result;
  }, [selectedCategory, selectedPrice, selectedSize]);

  const updateFilter = (key: string, value: string) => {
    const newParams = new URLSearchParams(searchParams);
    if (value) {
      newParams.set(key, value);
    } else {
      newParams.delete(key);
    }
    setSearchParams(newParams);
  };

  const clearFilters = () => {
    setSearchParams({});
  };

  const hasActiveFilters = selectedCategory || selectedPrice || selectedSize;

  return (
    <main className="pt-24 md:pt-28 pb-16">
      <div className="container-main">
        {/* Header */}
        <div className="mb-8">
          <h1 className="text-3xl md:text-5xl font-bold mb-4">Catálogo</h1>
          <p className="text-muted-foreground">
            {filteredProducts.length} produto{filteredProducts.length !== 1 ? 's' : ''} encontrado{filteredProducts.length !== 1 ? 's' : ''}
          </p>
        </div>

        <div className="flex gap-8">
          {/* Filters - Desktop */}
          <aside className="hidden lg:block w-64 flex-shrink-0">
            <div className="sticky top-28 space-y-8">
              <div className="flex items-center justify-between">
                <h3 className="font-bold text-lg">Filtros</h3>
                {hasActiveFilters && (
                  <button onClick={clearFilters} className="text-sm text-primary hover:underline">
                    Limpar
                  </button>
                )}
              </div>

              {/* Categories */}
              <div>
                <h4 className="font-semibold mb-4 text-sm uppercase tracking-wider">Categoria</h4>
                <div className="space-y-2">
                  {categories.map(cat => (
                    <button
                      key={cat.id}
                      onClick={() => updateFilter('categoria', selectedCategory === cat.id ? '' : cat.id)}
                      className={`block w-full text-left px-3 py-2 rounded-lg transition-colors ${
                        selectedCategory === cat.id
                          ? 'bg-primary text-primary-foreground'
                          : 'hover:bg-secondary'
                      }`}
                    >
                      {cat.icon} {cat.name}
                    </button>
                  ))}
                </div>
              </div>

              {/* Price */}
              <div>
                <h4 className="font-semibold mb-4 text-sm uppercase tracking-wider">Preço</h4>
                <div className="space-y-2">
                  {priceRanges.map(range => (
                    <button
                      key={range.id}
                      onClick={() => updateFilter('preco', selectedPrice === range.id ? '' : range.id)}
                      className={`block w-full text-left px-3 py-2 rounded-lg transition-colors ${
                        selectedPrice === range.id
                          ? 'bg-primary text-primary-foreground'
                          : 'hover:bg-secondary'
                      }`}
                    >
                      {range.label}
                    </button>
                  ))}
                </div>
              </div>

              {/* Size */}
              <div>
                <h4 className="font-semibold mb-4 text-sm uppercase tracking-wider">Tamanho</h4>
                <div className="flex flex-wrap gap-2">
                  {sizes.map(size => (
                    <button
                      key={size}
                      onClick={() => updateFilter('tamanho', selectedSize === size ? '' : size)}
                      className={`px-3 py-2 rounded border transition-colors ${
                        selectedSize === size
                          ? 'bg-primary text-primary-foreground border-primary'
                          : 'border-border hover:border-primary'
                      }`}
                    >
                      {size}
                    </button>
                  ))}
                </div>
              </div>
            </div>
          </aside>

          {/* Products Grid */}
          <div className="flex-1">
            {/* Mobile Filter Button */}
            <div className="lg:hidden mb-6 flex gap-4">
              <Button
                variant="outline"
                className="flex-1"
                onClick={() => setIsFilterOpen(true)}
              >
                <SlidersHorizontal className="w-4 h-4 mr-2" />
                Filtros
                {hasActiveFilters && (
                  <span className="ml-2 w-5 h-5 bg-primary text-primary-foreground rounded-full text-xs flex items-center justify-center">
                    !
                  </span>
                )}
              </Button>
            </div>

            {/* Active Filters Tags */}
            {hasActiveFilters && (
              <div className="flex flex-wrap gap-2 mb-6">
                {selectedCategory && (
                  <span className="inline-flex items-center gap-1 px-3 py-1 bg-secondary rounded-full text-sm">
                    {categories.find(c => c.id === selectedCategory)?.name}
                    <button onClick={() => updateFilter('categoria', '')}>
                      <X className="w-4 h-4" />
                    </button>
                  </span>
                )}
                {selectedPrice && (
                  <span className="inline-flex items-center gap-1 px-3 py-1 bg-secondary rounded-full text-sm">
                    {priceRanges.find(p => p.id === selectedPrice)?.label}
                    <button onClick={() => updateFilter('preco', '')}>
                      <X className="w-4 h-4" />
                    </button>
                  </span>
                )}
                {selectedSize && (
                  <span className="inline-flex items-center gap-1 px-3 py-1 bg-secondary rounded-full text-sm">
                    Tamanho {selectedSize}
                    <button onClick={() => updateFilter('tamanho', '')}>
                      <X className="w-4 h-4" />
                    </button>
                  </span>
                )}
              </div>
            )}

            {filteredProducts.length === 0 ? (
              <div className="text-center py-16">
                <p className="text-muted-foreground text-lg mb-4">
                  Nenhum produto encontrado com esses filtros.
                </p>
                <Button variant="outline" onClick={clearFilters}>
                  Limpar Filtros
                </Button>
              </div>
            ) : (
              <div className="grid grid-cols-2 md:grid-cols-3 gap-4 md:gap-6">
                {filteredProducts.map(product => (
                  <ProductCard key={product.id} product={product} />
                ))}
              </div>
            )}
          </div>
        </div>

        {/* Mobile Filter Sheet */}
        {isFilterOpen && (
          <div className="fixed inset-0 z-50 lg:hidden">
            <div className="absolute inset-0 bg-background/80 backdrop-blur-sm" onClick={() => setIsFilterOpen(false)} />
            <div className="absolute right-0 top-0 bottom-0 w-80 max-w-full bg-background border-l border-border p-6 overflow-y-auto animate-slide-in-right">
              <div className="flex items-center justify-between mb-8">
                <h3 className="font-bold text-lg">Filtros</h3>
                <button onClick={() => setIsFilterOpen(false)}>
                  <X className="w-6 h-6" />
                </button>
              </div>

              <div className="space-y-8">
                {/* Categories */}
                <div>
                  <h4 className="font-semibold mb-4 text-sm uppercase tracking-wider">Categoria</h4>
                  <div className="space-y-2">
                    {categories.map(cat => (
                      <button
                        key={cat.id}
                        onClick={() => {
                          updateFilter('categoria', selectedCategory === cat.id ? '' : cat.id);
                        }}
                        className={`block w-full text-left px-3 py-2 rounded-lg transition-colors ${
                          selectedCategory === cat.id
                            ? 'bg-primary text-primary-foreground'
                            : 'hover:bg-secondary'
                        }`}
                      >
                        {cat.icon} {cat.name}
                      </button>
                    ))}
                  </div>
                </div>

                {/* Price */}
                <div>
                  <h4 className="font-semibold mb-4 text-sm uppercase tracking-wider">Preço</h4>
                  <div className="space-y-2">
                    {priceRanges.map(range => (
                      <button
                        key={range.id}
                        onClick={() => {
                          updateFilter('preco', selectedPrice === range.id ? '' : range.id);
                        }}
                        className={`block w-full text-left px-3 py-2 rounded-lg transition-colors ${
                          selectedPrice === range.id
                            ? 'bg-primary text-primary-foreground'
                            : 'hover:bg-secondary'
                        }`}
                      >
                        {range.label}
                      </button>
                    ))}
                  </div>
                </div>

                {/* Size */}
                <div>
                  <h4 className="font-semibold mb-4 text-sm uppercase tracking-wider">Tamanho</h4>
                  <div className="flex flex-wrap gap-2">
                    {sizes.map(size => (
                      <button
                        key={size}
                        onClick={() => {
                          updateFilter('tamanho', selectedSize === size ? '' : size);
                        }}
                        className={`px-3 py-2 rounded border transition-colors ${
                          selectedSize === size
                            ? 'bg-primary text-primary-foreground border-primary'
                            : 'border-border hover:border-primary'
                        }`}
                      >
                        {size}
                      </button>
                    ))}
                  </div>
                </div>
              </div>

              <div className="mt-8 pt-6 border-t border-border space-y-3">
                <Button variant="hero" className="w-full" onClick={() => setIsFilterOpen(false)}>
                  Aplicar Filtros
                </Button>
                {hasActiveFilters && (
                  <Button variant="outline" className="w-full" onClick={clearFilters}>
                    Limpar Tudo
                  </Button>
                )}
              </div>
            </div>
          </div>
        )}
      </div>
    </main>
  );
};

export default Catalogo;
import { useState } from 'react';
import { MessageCircle, Instagram, MapPin, Mail } from 'lucide-react';
import { Button } from '@/components/ui/button';

const Contato = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: '',
  });

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    const message = encodeURIComponent(
      `*Contato via Site*

Nome: ${formData.name}
E-mail: ${formData.email}

Mensagem:
${formData.message}`
    );
    window.open(`https://wa.me/5511999999999?text=${message}`, '_blank');
  };

  return (
    <main className="pt-24 md:pt-28 pb-16">
      {/* Hero */}
      <section className="section-padding bg-secondary">
        <div className="container-main text-center">
          <span className="inline-block px-4 py-2 bg-primary text-primary-foreground text-sm font-bold uppercase tracking-wider mb-6">
            Fale com a gente
          </span>
          <h1 className="text-4xl md:text-6xl font-black mb-6">
            BORA TROCAR
            <br />
            <span className="text-gradient">UMA IDEIA?</span>
          </h1>
          <p className="text-lg md:text-xl text-muted-foreground max-w-2xl mx-auto">
            Dúvidas, sugestões ou só quer bater um papo? Cola junto que a gente responde rapidinho.
          </p>
        </div>
      </section>

      <section className="section-padding">
        <div className="container-main">
          <div className="grid md:grid-cols-2 gap-12 lg:gap-16">
            {/* Contact Info */}
            <div className="space-y-8">
              <div>
                <h2 className="text-2xl md:text-3xl font-bold mb-6">
                  Vem pro papo
                </h2>
                <p className="text-muted-foreground leading-relaxed">
                  O jeito mais rápido de falar com a gente é pelo WhatsApp. Respondemos todas as mensagens em até 24 horas (mas geralmente é bem mais rápido 😉).
                </p>
              </div>

              {/* WhatsApp Button */}
              <a
                href="https://wa.me/5511999999999"
                target="_blank"
                rel="noopener noreferrer"
                className="block"
              >
                <Button variant="whatsapp" size="xl" className="w-full sm:w-auto">
                  <MessageCircle className="w-5 h-5 mr-2" />
                  Chamar no WhatsApp
                </Button>
              </a>

              {/* Social Links */}
              <div className="pt-8 border-t border-border space-y-4">
                <h3 className="font-semibold">Nos siga nas redes</h3>
                <div className="flex gap-4">
                  <a
                    href="https://instagram.com"
                    target="_blank"
                    rel="noopener noreferrer"
                    className="flex items-center gap-2 px-4 py-3 bg-card rounded-lg hover:bg-secondary transition-colors"
                  >
                    <Instagram className="w-5 h-5 text-primary" />
                    <span>@urbanstore</span>
                  </a>
                  <a
                    href="https://tiktok.com"
                    target="_blank"
                    rel="noopener noreferrer"
                    className="flex items-center gap-2 px-4 py-3 bg-card rounded-lg hover:bg-secondary transition-colors"
                  >
                    <svg className="w-5 h-5 text-primary" viewBox="0 0 24 24" fill="currentColor">
                      <path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z" />
                    </svg>
                    <span>@urbanstore</span>
                  </a>
                </div>
              </div>

              {/* Other Info */}
              <div className="space-y-4">
                <div className="flex items-start gap-3">
                  <Mail className="w-5 h-5 text-primary mt-1" />
                  <div>
                    <p className="font-medium">E-mail</p>
                    <p className="text-muted-foreground">contato@urbanstore.com.br</p>
                  </div>
                </div>
                <div className="flex items-start gap-3">
                  <MapPin className="w-5 h-5 text-primary mt-1" />
                  <div>
                    <p className="font-medium">Localização</p>
                    <p className="text-muted-foreground">São Paulo, SP - Brasil</p>
                  </div>
                </div>
              </div>
            </div>

            {/* Contact Form */}
            <div className="bg-card p-6 md:p-8 rounded-lg">
              <h3 className="text-xl font-bold mb-6">Ou manda uma mensagem</h3>
              <form onSubmit={handleSubmit} className="space-y-4">
                <div>
                  <label htmlFor="name" className="block text-sm font-medium mb-2">
                    Nome
                  </label>
                  <input
                    type="text"
                    id="name"
                    required
                    value={formData.name}
                    onChange={(e) => setFormData(prev => ({ ...prev, name: e.target.value }))}
                    className="w-full px-4 py-3 bg-secondary border border-border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary"
                    placeholder="Seu nome"
                  />
                </div>
                <div>
                  <label htmlFor="email" className="block text-sm font-medium mb-2">
                    E-mail
                  </label>
                  <input
                    type="email"
                    id="email"
                    required
                    value={formData.email}
                    onChange={(e) => setFormData(prev => ({ ...prev, email: e.target.value }))}
                    className="w-full px-4 py-3 bg-secondary border border-border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary"
                    placeholder="seu@email.com"
                  />
                </div>
                <div>
                  <label htmlFor="message" className="block text-sm font-medium mb-2">
                    Mensagem
                  </label>
                  <textarea
                    id="message"
                    required
                    rows={5}
                    value={formData.message}
                    onChange={(e) => setFormData(prev => ({ ...prev, message: e.target.value }))}
                    className="w-full px-4 py-3 bg-secondary border border-border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary resize-none"
                    placeholder="Como podemos ajudar?"
                  />
                </div>
                <Button type="submit" variant="hero" size="lg" className="w-full">
                  Enviar via WhatsApp
                </Button>
              </form>
            </div>
          </div>
        </div>
      </section>
    </main>
  );
};

export default Contato;
import { useState } from 'react';
import { useParams, Link, useNavigate } from 'react-router-dom';
import { ArrowLeft, Minus, Plus, MessageCircle, Check } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { getProductById, products } from '@/data/products';
import { useCart } from '@/contexts/CartContext';
import ProductCard from '@/components/ProductCard';

const Produto = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  const { addToCart } = useCart();
  const product = getProductById(id || '');

  const [selectedSize, setSelectedSize] = useState('');
  const [selectedColor, setSelectedColor] = useState('');
  const [quantity, setQuantity] = useState(1);

  if (!product) {
    return (
      <main className="pt-24 md:pt-28 pb-16">
        <div className="container-main text-center py-16">
          <h1 className="text-3xl font-bold mb-4">Produto não encontrado</h1>
          <p className="text-muted-foreground mb-8">O produto que você procura não existe ou foi removido.</p>
          <Link to="/catalogo">
            <Button variant="hero">Ver Catálogo</Button>
          </Link>
        </div>
      </main>
    );
  }

  const hasDiscount = product.originalPrice && product.originalPrice > product.price;
  const relatedProducts = products.filter(p => p.category === product.category && p.id !== product.id).slice(0, 4);

  const handleAddToCart = () => {
    if (!selectedSize) {
      alert('Selecione um tamanho');
      return;
    }
    const color = selectedColor || product.colors[0].name;
    for (let i = 0; i < quantity; i++) {
      addToCart(product, selectedSize, color);
    }
  };

  const handleBuyWhatsApp = () => {
    if (!selectedSize) {
      alert('Selecione um tamanho');
      return;
    }
    const color = selectedColor || product.colors[0].name;
    const message = encodeURIComponent(
      `Olá! Tenho interesse no produto:\n\n` +
      `*${product.name}*\n` +
      `Tamanho: ${selectedSize}\n` +
      `Cor: ${color}\n` +
      `Quantidade: ${quantity}\n` +
      `Valor: R$ ${(product.price * quantity).toFixed(2).replace('.', ',')}\n\n` +
      `Link: ${window.location.href}`
    );
    window.open(`https://wa.me/5511999999999?text=${message}`, '_blank');
  };

  return (
    <main className="pt-24 md:pt-28 pb-16">
      <div className="container-main">
        {/* Breadcrumb */}
        <button
          onClick={() => navigate(-1)}
          className="inline-flex items-center gap-2 text-muted-foreground hover:text-foreground transition-colors mb-8"
        >
          <ArrowLeft className="w-4 h-4" />
          Voltar
        </button>

        <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 lg:gap-16">
          {/* Image Gallery */}
          <div className="space-y-4">
            <div className="aspect-square overflow-hidden rounded-lg bg-card">
              <img
                src={product.image}
                alt={product.name}
                className="w-full h-full object-cover"
              />
            </div>
            {product.images.length > 1 && (
              <div className="grid grid-cols-4 gap-2">
                {product.images.map((img, idx) => (
                  <button
                    key={idx}
                    className="aspect-square overflow-hidden rounded-lg bg-card border-2 border-transparent hover:border-primary transition-colors"
                  >
                    <img src={img} alt={`${product.name} ${idx + 1}`} className="w-full h-full object-cover" />
                  </button>
                ))}
              </div>
            )}
          </div>

          {/* Product Info */}
          <div className="space-y-6">
            {/* Badges */}
            <div className="flex gap-2">
              {product.isNew && (
                <span className="px-3 py-1 bg-primary text-primary-foreground text-xs font-bold uppercase">
                  Novo
                </span>
              )}
              {hasDiscount && (
                <span className="px-3 py-1 bg-destructive text-destructive-foreground text-xs font-bold uppercase">
                  Promoção
                </span>
              )}
            </div>

            <h1 className="text-3xl md:text-4xl font-bold">{product.name}</h1>

            {/* Price */}
            <div className="flex items-baseline gap-3">
              <span className="text-3xl font-bold text-primary">
                R$ {product.price.toFixed(2).replace('.', ',')}
              </span>
              {hasDiscount && (
                <span className="text-lg text-muted-foreground line-through">
                  R$ {product.originalPrice!.toFixed(2).replace('.', ',')}
                </span>
              )}
            </div>

            <p className="text-muted-foreground leading-relaxed">{product.description}</p>

            {/* Size Selection */}
            <div>
              <h4 className="font-semibold mb-3">Tamanho</h4>
              <div className="flex flex-wrap gap-2">
                {product.sizes.map(size => (
                  <button
                    key={size}
                    onClick={() => setSelectedSize(size)}
                    className={`min-w-[48px] px-4 py-2 rounded border-2 font-medium transition-all ${
                      selectedSize === size
                        ? 'bg-primary text-primary-foreground border-primary'
                        : 'border-border hover:border-primary'
                    }`}
                  >
                    {size}
                  </button>
                ))}
              </div>
            </div>

            {/* Color Selection */}
            {product.colors.length > 1 && (
              <div>
                <h4 className="font-semibold mb-3">Cor</h4>
                <div className="flex gap-3">
                  {product.colors.map(color => (
                    <button
                      key={color.name}
                      onClick={() => setSelectedColor(color.name)}
                      className={`w-10 h-10 rounded-full border-2 transition-all relative ${
                        selectedColor === color.name
                          ? 'border-primary scale-110'
                          : 'border-border hover:border-primary'
                      }`}
                      style={{ backgroundColor: color.hex }}
                      title={color.name}
                    >
                      {selectedColor === color.name && (
                        <Check className={`w-5 h-5 absolute inset-0 m-auto ${color.hex === '#000000' ? 'text-white' : 'text-black'}`} />
                      )}
                    </button>
                  ))}
                </div>
              </div>
            )}

            {/* Quantity */}
            <div>
              <h4 className="font-semibold mb-3">Quantidade</h4>
              <div className="inline-flex items-center border border-border rounded">
                <button
                  onClick={() => setQuantity(q => Math.max(1, q - 1))}
                  className="w-10 h-10 flex items-center justify-center hover:bg-secondary transition-colors"
                >
                  <Minus className="w-4 h-4" />
                </button>
                <span className="w-12 text-center font-medium">{quantity}</span>
                <button
                  onClick={() => setQuantity(q => q + 1)}
                  className="w-10 h-10 flex items-center justify-center hover:bg-secondary transition-colors"
                >
                  <Plus className="w-4 h-4" />
                </button>
              </div>
            </div>

            {/* Action Buttons */}
            <div className="flex flex-col sm:flex-row gap-4 pt-4">
              <Button
                variant="whatsapp"
                size="xl"
                className="flex-1"
                onClick={handleBuyWhatsApp}
              >
                <MessageCircle className="w-5 h-5 mr-2" />
                Comprar pelo WhatsApp
              </Button>
              <Button
                variant="hero"
                size="xl"
                className="flex-1"
                onClick={handleAddToCart}
              >
                Adicionar ao Carrinho
              </Button>
            </div>

            {/* Extra Info */}
            <div className="pt-6 border-t border-border space-y-3 text-sm">
              <p className="flex items-center gap-2 text-muted-foreground">
                <Check className="w-4 h-4 text-primary" />
                Frete grátis para compras acima de R$ 299
              </p>
              <p className="flex items-center gap-2 text-muted-foreground">
                <Check className="w-4 h-4 text-primary" />
                Troca garantida em até 30 dias
              </p>
              <p className="flex items-center gap-2 text-muted-foreground">
                <Check className="w-4 h-4 text-primary" />
                Pagamento seguro via WhatsApp
              </p>
            </div>
          </div>
        </div>

        {/* Related Products */}
        {relatedProducts.length > 0 && (
          <section className="mt-16 pt-16 border-t border-border">
            <h2 className="text-2xl md:text-3xl font-bold mb-8">Produtos Relacionados</h2>
            <div className="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-6">
              {relatedProducts.map(p => (
                <ProductCard key={p.id} product={p} />
              ))}
            </div>
          </section>
        )}
      </div>
    </main>
  );
};

export default Produto;
import { Link } from 'react-router-dom';
import { ArrowRight } from 'lucide-react';
import { Button } from '@/components/ui/button';

const Sobre = () => {
  return (
    <main className="pt-24 md:pt-28 pb-16">
      {/* Hero */}
      <section className="section-padding bg-secondary">
        <div className="container-main text-center">
          <span className="inline-block px-4 py-2 bg-primary text-primary-foreground text-sm font-bold uppercase tracking-wider mb-6">
            Nossa História
          </span>
          <h1 className="text-4xl md:text-6xl font-black mb-6">
            MAIS QUE ROUPA,
            <br />
            <span className="text-gradient">ATITUDE</span>
          </h1>
          <p className="text-lg md:text-xl text-muted-foreground max-w-2xl mx-auto">
            Nascemos nas ruas, crescemos com a cultura urbana e vestimos quem não tem medo de ser autêntico.
          </p>
        </div>
      </section>

      {/* Story */}
      <section className="section-padding">
        <div className="container-main">
          <div className="grid md:grid-cols-2 gap-12 items-center">
            <div>
              <h2 className="text-3xl md:text-4xl font-bold mb-6">
                De onde viemos
              </h2>
              <div className="space-y-4 text-muted-foreground leading-relaxed">
                <p>
                  A URBAN nasceu em 2020, no meio do caos, quando a gente percebeu que a moda mainstream não representava quem a gente era. Cansados das mesmas coisas de sempre, decidimos criar algo diferente.
                </p>
                <p>
                  Começamos pequeno, com estampas feitas à mão e vendendo pra amigos. Hoje, somos uma comunidade de milhares de pessoas que entendem que roupa é expressão, não só proteção.
                </p>
                <p>
                  Cada peça que criamos carrega a essência das ruas: autenticidade, ousadia e uma pitada de rebeldia. Não fazemos moda pra agradar todo mundo — fazemos pra quem entende.
                </p>
              </div>
            </div>
            <div className="aspect-square bg-card rounded-lg overflow-hidden">
              <div className="w-full h-full flex items-center justify-center bg-gradient-to-br from-primary/20 to-transparent">
                <span className="text-8xl font-black text-primary/30">U.</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* Values */}
      <section className="section-padding bg-secondary">
        <div className="container-main">
          <h2 className="text-3xl md:text-4xl font-bold text-center mb-12">
            No que acreditamos
          </h2>
          <div className="grid md:grid-cols-3 gap-8">
            {[
              {
                title: 'Autenticidade',
                desc: 'Ser você mesmo não é tendência, é estilo de vida. Nossas peças são pra quem não tem medo de mostrar quem é.',
                emoji: '🔥',
              },
              {
                title: 'Qualidade',
                desc: 'Cada detalhe importa. Usamos materiais premium porque a gente sabe que você merece peças que duram.',
                emoji: '✨',
              },
              {
                title: 'Comunidade',
                desc: 'Não vendemos roupa, construímos uma tribo. Quem veste URBAN faz parte de algo maior.',
                emoji: '🤝',
              },
            ].map(value => (
              <div
                key={value.title}
                className="bg-card p-8 rounded-lg card-hover text-center"
              >
                <span className="text-5xl mb-4 block">{value.emoji}</span>
                <h3 className="text-xl font-bold mb-3">{value.title}</h3>
                <p className="text-muted-foreground">{value.desc}</p>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* CTA */}
      <section className="section-padding bg-primary text-primary-foreground">
        <div className="container-main text-center">
          <h2 className="text-3xl md:text-4xl font-black mb-4">
            FAZ PARTE DA TRIBO
          </h2>
          <p className="text-lg opacity-90 mb-8 max-w-md mx-auto">
            Vista sua atitude. Confira nossa coleção e encontre peças que são a sua cara.
          </p>
          <Link to="/catalogo">
            <Button
              variant="outline"
              size="xl"
              className="border-primary-foreground text-primary-foreground hover:bg-primary-foreground hover:text-primary"
            >
              Ver Coleção
              <ArrowRight className="w-5 h-5 ml-2" />
            </Button>
          </Link>
        </div>
      </section>
    </main>
  );
};

export default Sobre;
import { Toaster } from "@/components/ui/toaster";
import { Toaster as Sonner } from "@/components/ui/sonner";
import { TooltipProvider } from "@/components/ui/tooltip";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { CartProvider } from "@/contexts/CartContext";
import Header from "@/components/Header";
import Footer from "@/components/Footer";
import WhatsAppButton from "@/components/WhatsAppButton";
import CartDrawer from "@/components/CartDrawer";
import Index from "./pages/Index";
import Catalogo from "./pages/Catalogo";
import Produto from "./pages/Produto";
import Sobre from "./pages/Sobre";
import Contato from "./pages/Contato";
import NotFound from "./pages/NotFound";

const queryClient = new QueryClient();

const App = () => (
  <QueryClientProvider client={queryClient}>
    <TooltipProvider>
      <CartProvider>
        <Toaster />
        <Sonner />
        <BrowserRouter>
          <Header />
          <CartDrawer />
          <Routes>
            <Route path="/" element={<Index />} />
            <Route path="/catalogo" element={<Catalogo />} />
            <Route path="/produto/:id" element={<Produto />} />
            <Route path="/sobre" element={<Sobre />} />
            <Route path="/contato" element={<Contato />} />
            <Route path="*" element={<NotFound />} />
          </Routes>
          <Footer />
          <WhatsAppButton />
        </BrowserRouter>
      </CartProvider>
    </TooltipProvider>
  </QueryClientProvider>
);

export default App;
import * as React from "react";
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";

import { cn } from "@/lib/utils";

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-semibold ring-offset-background transition-all duration-300 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:brightness-110 shadow-[0_4px_14px_-3px_hsla(48,100%,50%,0.4)] hover:shadow-[0_8px_30px_-8px_hsla(48,100%,50%,0.3)]",
        destructive: "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline: "border-2 border-primary text-primary bg-transparent hover:bg-primary hover:text-primary-foreground",
        secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
        hero: "bg-primary text-primary-foreground font-bold uppercase tracking-wider hover:brightness-110 shadow-[0_4px_20px_-3px_hsla(48,100%,50%,0.5)] hover:shadow-[0_8px_40px_-8px_hsla(48,100%,50%,0.4)] hover:-translate-y-0.5",
        whatsapp: "bg-[#25D366] text-foreground font-bold hover:brightness-110 shadow-[0_4px_14px_-3px_rgba(37,211,102,0.4)]",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-12 rounded-md px-8 text-base",
        xl: "h-14 rounded-md px-10 text-lg",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  },
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : "button";
    return <Comp className={cn(buttonVariants({ variant, size, className }))} ref={ref} {...props} />;
  },
);
Button.displayName = "Button";

export { Button, buttonVariants };
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* Streetwear Design System - Black & Yellow */
    --background: 0 0% 0%;
    --foreground: 0 0% 100%;

    --card: 0 0% 8%;
    --card-foreground: 0 0% 100%;

    --popover: 0 0% 8%;
    --popover-foreground: 0 0% 100%;

    /* Yellow accent - main brand color */
    --primary: 48 100% 50%;
    --primary-foreground: 0 0% 0%;

    --secondary: 0 0% 12%;
    --secondary-foreground: 0 0% 100%;

    --muted: 0 0% 15%;
    --muted-foreground: 0 0% 65%;

    --accent: 48 100% 50%;
    --accent-foreground: 0 0% 0%;

    --destructive: 0 84% 60%;
    --destructive-foreground: 0 0% 100%;

    --border: 0 0% 20%;
    --input: 0 0% 15%;
    --ring: 48 100% 50%;

    --radius: 0.5rem;

    /* Custom brand tokens */
    --brand-black: 0 0% 0%;
    --brand-dark: 0 0% 12%;
    --brand-yellow: 48 100% 50%;
    --brand-white: 0 0% 100%;
    --brand-gray: 0 0% 40%;

    /* Gradients */
    --gradient-hero: linear-gradient(135deg, hsl(0 0% 0%) 0%, hsl(0 0% 8%) 100%);
    --gradient-card: linear-gradient(180deg, hsl(0 0% 10%) 0%, hsl(0 0% 5%) 100%);
    --gradient-yellow: linear-gradient(135deg, hsl(48 100% 50%) 0%, hsl(45 100% 45%) 100%);

    /* Shadows */
    --shadow-card: 0 4px 20px -4px hsla(0, 0%, 0%, 0.5);
    --shadow-button: 0 4px 14px -3px hsla(48, 100%, 50%, 0.4);
    --shadow-hover: 0 8px 30px -8px hsla(48, 100%, 50%, 0.3);

    --sidebar-background: 0 0% 5%;
    --sidebar-foreground: 0 0% 100%;
    --sidebar-primary: 48 100% 50%;
    --sidebar-primary-foreground: 0 0% 0%;
    --sidebar-accent: 0 0% 15%;
    --sidebar-accent-foreground: 0 0% 100%;
    --sidebar-border: 0 0% 20%;
    --sidebar-ring: 48 100% 50%;
  }
}

@layer base {
  * {
    @apply border-border;
  }

  html {
    scroll-behavior: smooth;
  }

  body {
    @apply bg-background text-foreground antialiased;
    font-family: 'Poppins', sans-serif;
  }

  h1, h2, h3, h4, h5, h6 {
    @apply font-bold tracking-tight;
  }
}

@layer components {
  .text-gradient {
    @apply bg-clip-text text-transparent;
    background-image: var(--gradient-yellow);
  }

  .card-hover {
    @apply transition-all duration-300 ease-out;
  }

  .card-hover:hover {
    @apply -translate-y-1;
    box-shadow: var(--shadow-hover);
  }

  .btn-primary {
    @apply bg-primary text-primary-foreground font-semibold;
    box-shadow: var(--shadow-button);
  }

  .btn-primary:hover {
    @apply brightness-110;
    box-shadow: var(--shadow-hover);
  }

  .section-padding {
    @apply px-4 py-16 md:px-8 lg:px-16 lg:py-24;
  }

  .container-main {
    @apply max-w-7xl mx-auto px-4 md:px-8;
  }
}

@layer utilities {
  .animate-float {
    animation: float 3s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-10px);
    }
  }

  .animate-pulse-slow {
    animation: pulse 3s ease-in-out infinite;
  }

  .animate-slide-up {
    animation: slideUp 0.6s ease-out forwards;
  }

  @keyframes slideUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .animate-fade-in {
    animation: fadeIn 0.5s ease-out forwards;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
}
import { Link } from 'react-router-dom';
import { ArrowRight, Zap, Truck, Shield } from 'lucide-react';
import { Button } from '@/components/ui/button';
import ProductCard from '@/components/ProductCard';
import { getNewProducts, getBestSellers, getPromotions, categories } from '@/data/products';
import heroBanner from '@/assets/hero-banner.jpg';

const Index = () => {
  const newProducts = getNewProducts();
  const bestSellers = getBestSellers();
  const promotions = getPromotions();

  return (
    <main className="pt-16 md:pt-20">
      {/* Hero Section */}
      <section className="relative h-[90vh] md:h-screen flex items-center overflow-hidden">
        {/* Background Image */}
        <div className="absolute inset-0">
          <img
            src={heroBanner}
            alt="Streetwear fashion"
            className="w-full h-full object-cover object-center"
          />
          <div className="absolute inset-0 bg-gradient-to-r from-background via-background/80 to-transparent" />
        </div>

        {/* Content */}
        <div className="container-main relative z-10">
          <div className="max-w-2xl animate-slide-up">
            <span className="inline-block px-4 py-2 bg-primary text-primary-foreground text-sm font-bold uppercase tracking-wider mb-6">
              Nova Coleção 2025
            </span>
            <h1 className="text-4xl md:text-6xl lg:text-7xl font-black leading-none mb-6">
              VISTA SUA
              <br />
              <span className="text-gradient">ATITUDE</span>
            </h1>
            <p className="text-lg md:text-xl text-muted-foreground mb-8 max-w-md">
              Moda urbana autêntica para quem não segue tendências — cria as próprias.
            </p>
            <div className="flex flex-col sm:flex-row gap-4">
              <Link to="/catalogo">
                <Button variant="hero" size="xl">
                  Comprar Agora
                  <ArrowRight className="w-5 h-5 ml-2" />
                </Button>
              </Link>
              <Link to="/catalogo">
                <Button variant="outline" size="xl">
                  Ver Coleção
                </Button>
              </Link>
            </div>
          </div>
        </div>

        {/* Scroll indicator */}
        <div className="absolute bottom-8 left-1/2 -translate-x-1/2 animate-bounce">
          <div className="w-6 h-10 border-2 border-foreground/30 rounded-full flex items-start justify-center p-2">
            <div className="w-1 h-3 bg-primary rounded-full" />
          </div>
        </div>
      </section>

      {/* Features Bar */}
      <section className="bg-secondary py-6 border-y border-border">
        <div className="container-main">
          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {[
              { icon: Truck, title: 'Frete Grátis', desc: 'Compras acima de R$ 299' },
              { icon: Zap, title: 'Envio Rápido', desc: 'Entrega em até 5 dias' },
              { icon: Shield, title: 'Compra Segura', desc: 'Seus dados protegidos' },
            ].map(({ icon: Icon, title, desc }) => (
              <div key={title} className="flex items-center gap-4">
                <div className="w-12 h-12 bg-primary/10 rounded-full flex items-center justify-center">
                  <Icon className="w-6 h-6 text-primary" />
                </div>
                <div>
                  <h4 className="font-semibold">{title}</h4>
                  <p className="text-sm text-muted-foreground">{desc}</p>
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Categories */}
      <section className="section-padding">
        <div className="container-main">
          <div className="text-center mb-12">
            <h2 className="text-3xl md:text-4xl font-bold mb-4">Explore por Categoria</h2>
            <p className="text-muted-foreground">Encontre o que combina com seu estilo</p>
          </div>
          <div className="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-7 gap-4">
            {categories.map(cat => (
              <Link
                key={cat.id}
                to={`/catalogo?categoria=${cat.id}`}
                className="group flex flex-col items-center p-6 bg-card rounded-lg card-hover"
              >
                <span className="text-4xl mb-3">{cat.icon}</span>
                <span className="font-medium text-sm group-hover:text-primary transition-colors">
                  {cat.name}
                </span>
              </Link>
            ))}
          </div>
        </div>
      </section>

      {/* New Arrivals */}
      <section className="section-padding bg-secondary">
        <div className="container-main">
          <div className="flex items-center justify-between mb-12">
            <div>
              <h2 className="text-3xl md:text-4xl font-bold mb-2">Novidades</h2>
              <p className="text-muted-foreground">Os lançamentos mais quentes</p>
            </div>
            <Link to="/catalogo" className="hidden md:flex items-center gap-2 text-primary font-medium hover:underline">
              Ver Todos
              <ArrowRight className="w-4 h-4" />
            </Link>
          </div>
          <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-6">
            {newProducts.slice(0, 4).map(product => (
              <ProductCard key={product.id} product={product} />
            ))}
          </div>
          <Link to="/catalogo" className="md:hidden flex items-center justify-center gap-2 mt-8 text-primary font-medium">
            Ver Todos
            <ArrowRight className="w-4 h-4" />
          </Link>
        </div>
      </section>

      {/* Best Sellers */}
      <section className="section-padding">
        <div className="container-main">
          <div className="flex items-center justify-between mb-12">
            <div>
              <h2 className="text-3xl md:text-4xl font-bold mb-2">Mais Vendidos</h2>
              <p className="text-muted-foreground">O que a galera tá usando</p>
            </div>
            <Link to="/catalogo" className="hidden md:flex items-center gap-2 text-primary font-medium hover:underline">
              Ver Todos
              <ArrowRight className="w-4 h-4" />
            </Link>
          </div>
          <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-6">
            {bestSellers.slice(0, 4).map(product => (
              <ProductCard key={product.id} product={product} />
            ))}
          </div>
        </div>
      </section>

      {/* Promo Banner */}
      <section className="section-padding bg-primary text-primary-foreground">
        <div className="container-main text-center">
          <span className="inline-block px-4 py-2 bg-primary-foreground text-primary text-sm font-bold uppercase tracking-wider mb-6">
            Promoção Limitada
          </span>
          <h2 className="text-3xl md:text-5xl font-black mb-4">
            ATÉ 40% OFF
          </h2>
          <p className="text-lg opacity-90 mb-8 max-w-md mx-auto">
            Peças selecionadas com descontos imperdíveis. Corre que é por tempo limitado!
          </p>
          <Link to="/catalogo">
            <Button
              variant="outline"
              size="xl"
              className="border-primary-foreground text-primary-foreground hover:bg-primary-foreground hover:text-primary"
            >
              Ver Ofertas
              <ArrowRight className="w-5 h-5 ml-2" />
            </Button>
          </Link>
        </div>
      </section>

      {/* Promotions */}
      {promotions.length > 0 && (
        <section className="section-padding">
          <div className="container-main">
            <div className="flex items-center justify-between mb-12">
              <div>
                <h2 className="text-3xl md:text-4xl font-bold mb-2">Em Promoção</h2>
                <p className="text-muted-foreground">Aproveite os melhores preços</p>
              </div>
            </div>
            <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-6">
              {promotions.map(product => (
                <ProductCard key={product.id} product={product} />
              ))}
            </div>
          </div>
        </section>
      )}

      {/* Newsletter */}
      <section className="section-padding bg-secondary">
        <div className="container-main text-center">
          <h2 className="text-3xl md:text-4xl font-bold mb-4">
            Fique por Dentro
          </h2>
          <p className="text-muted-foreground mb-8 max-w-md mx-auto">
            Cadastre-se e receba em primeira mão as novidades e promoções exclusivas.
          </p>
          <form className="max-w-md mx-auto flex flex-col sm:flex-row gap-3">
            <input
              type="email"
              placeholder="Seu melhor e-mail"
              className="flex-1 px-4 py-3 bg-card border border-border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary"
            />
            <Button variant="hero" size="lg">
              Cadastrar
            </Button>
          </form>
        </div>
      </section>
    </main>
  );
};

export default Index;
import { Link, useLocation } from 'react-router-dom';
import { useEffect } from 'react';
import { ArrowLeft } from 'lucide-react';
import { Button } from '@/components/ui/button';

const NotFound = () => {
  const location = useLocation();

  useEffect(() => {
    console.error("404 Error: User attempted to access non-existent route:", location.pathname);
  }, [location.pathname]);

  return (
    <main className="pt-24 md:pt-28 pb-16 min-h-screen flex items-center">
      <div className="container-main text-center">
        <span className="text-8xl md:text-9xl font-black text-primary">404</span>
        <h1 className="text-3xl md:text-4xl font-bold mt-4 mb-4">
          Página não encontrada
        </h1>
        <p className="text-muted-foreground mb-8 max-w-md mx-auto">
          Parece que você se perdeu nas ruas. Essa página não existe ou foi movida.
        </p>
        <Link to="/">
          <Button variant="hero" size="lg">
            <ArrowLeft className="w-4 h-4 mr-2" />
            Voltar pro Início
          </Button>
        </Link>
      </div>
    </main>
  );
};

export default NotFound;
import type { Config } from "tailwindcss";

export default {
  darkMode: ["class"],
  content: ["./pages/**/*.{ts,tsx}", "./components/**/*.{ts,tsx}", "./app/**/*.{ts,tsx}", "./src/**/*.{ts,tsx}"],
  prefix: "",
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: {
        "2xl": "1400px",
      },
    },
    extend: {
      fontFamily: {
        sans: ['Poppins', 'sans-serif'],
      },
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))",
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
        brand: {
          black: "hsl(var(--brand-black))",
          dark: "hsl(var(--brand-dark))",
          yellow: "hsl(var(--brand-yellow))",
          white: "hsl(var(--brand-white))",
          gray: "hsl(var(--brand-gray))",
        },
        sidebar: {
          DEFAULT: "hsl(var(--sidebar-background))",
          foreground: "hsl(var(--sidebar-foreground))",
          primary: "hsl(var(--sidebar-primary))",
          "primary-foreground": "hsl(var(--sidebar-primary-foreground))",
          accent: "hsl(var(--sidebar-accent))",
          "accent-foreground": "hsl(var(--sidebar-accent-foreground))",
          border: "hsl(var(--sidebar-border))",
          ring: "hsl(var(--sidebar-ring))",
        },
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" },
        },
        "slide-in-right": {
          from: { transform: "translateX(100%)", opacity: "0" },
          to: { transform: "translateX(0)", opacity: "1" },
        },
        "slide-in-left": {
          from: { transform: "translateX(-100%)", opacity: "0" },
          to: { transform: "translateX(0)", opacity: "1" },
        },
        "scale-in": {
          from: { transform: "scale(0.95)", opacity: "0" },
          to: { transform: "scale(1)", opacity: "1" },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
        "slide-in-right": "slide-in-right 0.4s ease-out",
        "slide-in-left": "slide-in-left 0.4s ease-out",
        "scale-in": "scale-in 0.3s ease-out",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
} satisfies Config;