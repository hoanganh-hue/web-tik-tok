# web-tik-tokTikTok Shop - Tài liệu hướng dẫn phát triển hệ thống
Mục lục
Tổng quan hệ thống
Sơ đồ cấu trúc dự án
Kiến trúc kỹ thuật
Phần I: Ứng dụng thương mại điện tử
1. Thiết lập cơ sở dữ liệu
2. Xây dựng API Backend
3. Phát triển Frontend
4. Chức năng người dùng chính
5. Chức năng người bán hàng
6. Tích hợp thanh toán
7. Hệ thống tiếp thị liên kết và mã giới thiệu
Phần II: Hệ thống quản trị (Admin)
1. Kiến trúc hậu đài
2. Quản lý người dùng và người bán
3. Quản lý sản phẩm
4. Quản lý đơn hàng
5. Quản lý giao dịch tài chính
6. Dữ liệu thống kê và báo cáo
7. Tạo dữ liệu mẫu
Triển khai và vận hành
Bảo mật hệ thống
Tổng quan hệ thống
TikTok Shop là một nền tảng thương mại điện tử toàn diện, phát triển dựa trên mô hình sàn thương mại điện tử (marketplace). Hệ thống bao gồm hai phần chính:

Ứng dụng thương mại điện tử: Giao diện chính dành cho người dùng cuối và người bán hàng, cung cấp đầy đủ các chức năng mua bán, quản lý sản phẩm, ví điện tử và tiếp thị liên kết.

Hệ thống quản trị (Admin): Giao diện hậu đài dành cho quản trị viên, cung cấp các công cụ kiểm soát toàn bộ nền tảng, phê duyệt người bán và sản phẩm, và theo dõi hoạt động kinh doanh.

Đặc điểm nổi bật:
Thiết kế đáp ứng: Tối ưu hóa cho cả máy tính và thiết bị di động
Hệ thống đa người dùng: Phân quyền rõ ràng giữa người dùng, người bán và quản trị viên
Hệ thống tiếp thị liên kết: Hỗ trợ đăng ký người bán thông qua mã giới thiệu
Ví điện tử tích hợp: Quản lý thu nhập và rút tiền cho người bán
Phê duyệt nội dung: Kiểm soát người bán và sản phẩm trước khi hiển thị trên sàn
Hệ thống tạo cửa hàng mẫu: Tạo dữ liệu mẫu chất lượng cao để làm phong phú giao diện
Sơ đồ cấu trúc dự án
tiktok-shop/
├── client/                     # Frontend React application
│   ├── src/
│   │   ├── assets/             # Hình ảnh, biểu tượng và tài nguyên tĩnh
│   │   ├── components/         # React components tái sử dụng
│   │   │   ├── ui/             # Các thành phần giao diện cơ bản (shadcn)
│   │   │   ├── home/           # Components cho trang chủ
│   │   │   ├── product/        # Components liên quan đến sản phẩm
│   │   │   ├── auth/           # Components xác thực
│   │   │   ├── admin/          # Components cho giao diện admin
│   │   │   └── seller/         # Components cho người bán
│   │   ├── hooks/              # React custom hooks
│   │   ├── lib/                # Thư viện tiện ích và helpers
│   │   ├── pages/              # Trang giao diện
│   │   │   ├── admin/          # Giao diện admin
│   │   │   ├── seller/         # Giao diện người bán
│   │   │   └── ...             # Các trang người dùng khác
│   │   ├── App.tsx             # Component gốc và định tuyến
│   │   └── main.tsx            # Điểm khởi đầu ứng dụng
│   └── ...
├── server/                     # Backend Express application
│   ├── admin-api.ts            # API dành riêng cho admin
│   ├── auth.ts                 # Xác thực người dùng
│   ├── db.ts                   # Kết nối cơ sở dữ liệu
│   ├── index.ts                # Điểm khởi đầu ứng dụng server
│   ├── routes.ts               # Định nghĩa API routes
│   └── storage.ts              # Lớp truy cập dữ liệu
├── shared/                     # Mã nguồn dùng chung
│   └── schema.ts               # Định nghĩa schema và models
├── utils/                      # Công cụ hỗ trợ
│   ├── sample-products-creator.js  # Tạo sản phẩm mẫu
│   └── ...
├── public/                     # Tài nguyên tĩnh
├── package.json                # Cấu hình dự án và dependencies
└── ...
Kiến trúc kỹ thuật
Stack công nghệ:
Frontend:

React.js với TypeScript
Tailwind CSS + shadcn/ui cho giao diện
TanStack React Query cho quản lý trạng thái và fetching data
Wouter cho định tuyến
Zod cho validation dữ liệu client-side
Backend:

Node.js với Express
Passport.js cho xác thực
JWT (JSON Web Token) cho bảo mật API
TypeScript cho type-safety
Database:

PostgreSQL
Drizzle ORM cho tương tác cơ sở dữ liệu
Drizzle + Zod cho validation schema
Công cụ phát triển:

Vite cho bundling và development server
ESLint và Prettier cho code style
Kiến trúc tổng thể:
Kiến trúc Monorepo: Frontend và Backend được phát triển trong cùng một repository, chia sẻ model và type chung thông qua thư mục shared.

API Pattern: RESTful API được sử dụng để giao tiếp giữa Frontend và Backend.

MVCS Pattern (Model-View-Controller-Service):

Model: Định nghĩa trong shared/schema.ts
View: React components trong thư mục client/src/components và client/src/pages
Controller: API routes trong server/routes.ts và server/admin-api.ts
Service: Logic xử lý dữ liệu trong server/storage.ts
Làm việc với Database: Sử dụng Drizzle ORM với Repository Pattern thông qua storage.ts.

Phần I: Ứng dụng thương mại điện tử
1. Thiết lập cơ sở dữ liệu
1.1. Schema và Relations
File shared/schema.ts định nghĩa toàn bộ cấu trúc dữ liệu của hệ thống:

// Các bảng chính trong hệ thống
export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  username: text('username').notNull().unique(),
  email: text('email').notNull().unique(),
  password: text('password').notNull(),
  role: text('role', { enum: ['user', 'seller', 'admin'] }).default('user').notNull(),
  lastLogin: timestamp('last_login'),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow()
});
export const products = pgTable('products', {
  id: serial('id').primaryKey(),
  name: text('name').notNull(),
  slug: text('slug').notNull().unique(),
  description: text('description'),
  price: numeric('price').notNull(),
  discountedPrice: numeric('discounted_price'),
  isDiscounted: boolean('is_discounted').default(false),
  stock: integer('stock').notNull().default(0),
  categoryId: integer('category_id').references(() => categories.id),
  sellerId: integer('seller_id').references(() => sellers.id),
  status: text('status', { enum: ['pending', 'approved', 'rejected'] }).default('pending'),
  isFeatured: boolean('is_featured').default(false),
  isSampleStore: boolean('is_sample_store').default(false),
  isDeleted: boolean('is_deleted').default(false),
  image: text('image'),
  sold: integer('sold').default(0),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow()
});
// Và các bảng khác như: categories, sellers, orders, wallets, withdrawals, v.v.
1.2. Cấu hình kết nối Database
File server/db.ts thiết lập kết nối PostgreSQL và export đối tượng Drizzle:

import { drizzle } from 'drizzle-orm/postgres-js';
import postgres from 'postgres';
import * as schema from '@shared/schema';
export const pool = new Pool({ 
  connectionString: process.env.DATABASE_URL 
});
export const db = drizzle(pool, { schema });
export async function checkConnection() {
  try {
    await pool.query('SELECT NOW()');
    console.log('Database connection successful');
  } catch (error) {
    console.error('Database connection failed:', error);
    throw error;
  }
}
1.3. Migrations
Sử dụng Drizzle Kit để quản lý migrations:

# Tạo migration file
npm run db:generate
# Apply migrations
npm run db:push
2. Xây dựng API Backend
2.1. Lớp truy cập dữ liệu (Storage)
File server/storage.ts triển khai tất cả các phương thức truy cập và thao tác dữ liệu:

export interface IStorage {
  // Users
  getUser(id: number): Promise<User | undefined>;
  getUserByUsername(username: string): Promise<User | undefined>;
  // ... các phương thức khác
  // Products
  getProduct(id: number): Promise<Product | undefined>;
  createProduct(product: InsertProduct): Promise<Product>;
  // ... các phương thức khác
  // Orders, Wallets, etc.
  // ...
}
export class DatabaseStorage implements IStorage {
  // Triển khai các phương thức
  async getUser(id: number): Promise<User | undefined> {
    const [user] = await db.select().from(users).where(eq(users.id, id));
    return user;
  }
  
  // ... và các phương thức khác
}
2.2. API Routes
File server/routes.ts định nghĩa toàn bộ API endpoints:

export async function registerRoutes(app: Express): Promise<Server> {
  // Xác thực
  setupAuth(app);
  
  // Danh mục
  app.get("/api/categories", async (req, res) => {
    try {
      const categories = await storage.getAllCategories();
      res.json(categories);
    } catch (error) {
      res.status(500).json({ message: "Error fetching categories", error });
    }
  });
  
  // Sản phẩm
  app.get("/api/products", async (req, res) => { /* ... */ });
  app.post("/api/products", sellerMiddleware, async (req, res) => { /* ... */ });
  
  // Đơn hàng
  app.get("/api/orders", authMiddleware, async (req, res) => { /* ... */ });
  app.post("/api/orders", authMiddleware, async (req, res) => { /* ... */ });
  
  // Và các routes khác
  // ...
  
  // Khởi tạo HTTP server
  const httpServer = createServer(app);
  return httpServer;
}
2.3. Xác thực và bảo mật
File server/auth.ts xử lý đăng nhập, đăng ký và phân quyền:

export function setupAuth(app: Express) {
  // Cấu hình Passport.js
  passport.use(new LocalStrategy(async (username, password, done) => {
    // Logic xác thực
  }));
  
  // API đăng ký
  app.post("/api/register", async (req, res, next) => {
    // Xử lý đăng ký người dùng
  });
  
  // API đăng nhập
  app.post("/api/login", (req, res, next) => {
    passport.authenticate("local", (err, user, info) => {
      // Xử lý đăng nhập
    })(req, res, next);
  });
  
  // Middleware kiểm tra quyền truy cập
  const authMiddleware = (req, res, next) => { /* ... */ };
  const sellerMiddleware = (req, res, next) => { /* ... */ };
  const adminMiddleware = (req, res, next) => { /* ... */ };
}
3. Phát triển Frontend
3.1. Cấu trúc giao diện
Ứng dụng được xây dựng với các thành phần chính:

Layout: Bố cục chung bao gồm header, navigation, footer
Routing: Sử dụng Wouter để định tuyến giữa các trang
Components: Các thành phần UI tái sử dụng
Hooks: Custom hooks để quản lý logic chung
Theme: Sử dụng Tailwind CSS và theme.json để định nghĩa styles
3.2. State Management và Data Fetching
Sử dụng TanStack React Query để quản lý trạng thái và fetching data:

// Lấy danh sách danh mục
const { data: categories, isLoading } = useQuery({
  queryKey: ['/api/categories'],
  queryFn: getQueryFn()
});
// Thêm sản phẩm mới
const addProductMutation = useMutation({
  mutationFn: async (data: InsertProduct) => {
    return apiRequest('POST', '/api/products', data);
  },
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['/api/products'] });
    toast({ title: "Thêm sản phẩm thành công" });
  }
});
3.3. Xác thực người dùng
Triển khai hook useAuth để quản lý trạng thái xác thực:

export function useAuth() {
  const { data: user, isLoading } = useQuery({
    queryKey: ['/api/user'],
    queryFn: getQueryFn({ on401: 'returnNull' })
  });
  
  const loginMutation = useMutation({
    mutationFn: async (credentials: LoginData) => {
      const res = await apiRequest('POST', '/api/login', credentials);
      return res.json();
    },
    onSuccess: (user) => {
      queryClient.setQueryData(['/api/user'], user);
    }
  });
  
  // Đăng ký, đăng xuất, v.v.
  
  return { user, isLoading, loginMutation, ... };
}
4. Chức năng người dùng chính
4.1. Đăng ký và đăng nhập
Trang auth-page.tsx cung cấp form đăng ký và đăng nhập:

function AuthPage() {
  const { user, loginMutation, registerMutation } = useAuth();
  const navigate = useNavigate();
  
  // Redirect if already logged in
  if (user) {
    navigate("/");
    return null;
  }
  
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 h-screen">
      <div className="p-8">
        <Tabs defaultValue="login">
          <TabsList>
            <TabsTrigger value="login">Đăng nhập</TabsTrigger>
            <TabsTrigger value="register">Đăng ký</TabsTrigger>
          </TabsList>
          
          <TabsContent value="login">
            <LoginForm onSubmit={(data) => loginMutation.mutate(data)} />
          </TabsContent>
          
          <TabsContent value="register">
            <RegisterForm onSubmit={(data) => registerMutation.mutate(data)} />
          </TabsContent>
        </Tabs>
      </div>
      
      <div className="bg-primary hidden md:block">
        {/* Hero section */}
      </div>
    </div>
  );
}
4.2. Trang chủ và hiển thị sản phẩm
Trang chủ (home-page.tsx) hiển thị các danh mục, sản phẩm nổi bật và khuyến mãi:

function HomePage() {
  return (
    <div>
      <HeroBanner />
      <CategorySection />
      <FeaturedProducts />
      <BestSellers />
      <DiscountedProducts />
    </div>
  );
}
4.3. Trang danh mục và tìm kiếm
Trang danh mục (category-page.tsx) hiển thị sản phẩm theo danh mục và hỗ trợ bộ lọc:

function CategoryPage() {
  const params = useParams();
  const slug = params.slug;
  
  const { data: category } = useQuery({
    queryKey: [`/api/categories/${slug}`],
    queryFn: getQueryFn()
  });
  
  const { data: products } = useQuery({
    queryKey: [`/api/products/category/${category?.id}`],
    queryFn: getQueryFn(),
    enabled: !!category?.id
  });
  
  // Render UI with filtering and sorting options
}
4.4. Chi tiết sản phẩm
Trang chi tiết sản phẩm (product-page.tsx) hiển thị thông tin đầy đủ và cho phép thêm vào giỏ hàng:

function ProductPage() {
  const params = useParams();
  const slug = params.slug;
  
  const { data: product } = useQuery({
    queryKey: [`/api/products/slug/${slug}`],
    queryFn: getQueryFn()
  });
  
  const { data: reviews } = useQuery({
    queryKey: [`/api/reviews/product/${product?.id}`],
    queryFn: getQueryFn(),
    enabled: !!product?.id
  });
  
  const addToCartMutation = useMutation({
    mutationFn: async (data) => {
      return apiRequest('POST', '/api/cart/items', data);
    },
    onSuccess: () => {
      toast({ title: "Đã thêm vào giỏ hàng" });
    }
  });
  
  // Render product details, images, reviews, add to cart button
}
4.5. Giỏ hàng và thanh toán
Trang giỏ hàng (cart-page.tsx) và thanh toán (checkout-page.tsx) quản lý quá trình mua hàng:

function CartPage() {
  const { data: cart } = useQuery({
    queryKey: ['/api/cart'],
    queryFn: getQueryFn()
  });
  
  const updateItemMutation = useMutation({
    mutationFn: async ({ id, quantity }) => {
      return apiRequest('PATCH', `/api/cart/items/${id}`, { quantity });
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/cart'] });
    }
  });
  
  const removeItemMutation = useMutation({
    mutationFn: async (id) => {
      return apiRequest('DELETE', `/api/cart/items/${id}`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/cart'] });
    }
  });
  
  // Render cart items, totals, and checkout button
}
5. Chức năng người bán hàng
5.1. Đăng ký và xác minh người bán
Trang đăng ký người bán (seller-register.tsx) cho phép người dùng đăng ký làm người bán:

function SellerRegisterPage() {
  const { user } = useAuth();
  const [referralCode, setReferralCode] = useState('');
  
  const registerSellerMutation = useMutation({
    mutationFn: async (data) => {
      return apiRequest('POST', '/api/sellers', {
        ...data,
        referralCode: referralCode || undefined
      });
    },
    onSuccess: () => {
      toast({ title: "Đăng ký thành công, chờ xác minh" });
      navigate("/seller/dashboard");
    }
  });
  
  // Render seller registration form with referral code
}
5.2. Quản lý sản phẩm
Trang quản lý sản phẩm (seller/products.tsx) cho phép người bán thêm, sửa, xóa sản phẩm:

function SellerProductsPage() {
  const { data: products } = useQuery({
    queryKey: ['/api/products', { sellerId: true }],
    queryFn: getQueryFn()
  });
  
  // Add product modal
  const [isAddOpen, setIsAddOpen] = useState(false);
  
  const addProductMutation = useMutation({
    mutationFn: async (data) => {
      return apiRequest('POST', '/api/products', data);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/products'] });
      setIsAddOpen(false);
    }
  });
  
  // Render product list, add button, edit and delete actions
}
5.3. Quản lý đơn hàng
Trang quản lý đơn hàng (seller/orders.tsx) hiển thị và cập nhật trạng thái đơn hàng:

function SellerOrdersPage() {
  const { data: seller } = useQuery({
    queryKey: ['/api/sellers/user'],
    queryFn: getQueryFn()
  });
  
  const { data: orders } = useQuery({
    queryKey: ['/api/orders/seller'],
    queryFn: getQueryFn(),
    enabled: !!seller?.id
  });
  
  const updateOrderMutation = useMutation({
    mutationFn: async ({ id, status }) => {
      return apiRequest('PATCH', `/api/orders/${id}`, { status });
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/orders/seller'] });
    }
  });
  
  // Render orders with filtering and status update actions
}
5.4. Ví điện tử và rút tiền
Trang ví điện tử (seller/wallet.tsx) hiển thị số dư và cho phép rút tiền:

function SellerWalletPage() {
  const { data: wallet } = useQuery({
    queryKey: ['/api/wallet'],
    queryFn: getQueryFn()
  });
  
  const { data: transactions } = useQuery({
    queryKey: ['/api/wallet/transactions'],
    queryFn: getQueryFn()
  });
  
  const { data: banking } = useQuery({
    queryKey: ['/api/banking'],
    queryFn: getQueryFn({ on404: 'returnNull' })
  });
  
  const withdrawMutation = useMutation({
    mutationFn: async (data) => {
      return apiRequest('POST', '/api/withdrawals', data);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/wallet'] });
      queryClient.invalidateQueries({ queryKey: ['/api/wallet/transactions'] });
    }
  });
  
  // Render wallet balance, transactions, withdrawal form
}
5.5. Tiếp thị liên kết
Trang tiếp thị liên kết (seller/affiliate.tsx) hiển thị mã giới thiệu và sản phẩm có thể tiếp thị:

function SellerAffiliatePage() {
  const { data: seller } = useQuery({
    queryKey: ['/api/sellers/user'],
    queryFn: getQueryFn()
  });
  
  const { data: affiliateProducts } = useQuery({
    queryKey: ['/api/products/affiliate'],
    queryFn: getQueryFn()
  });
  
  const addAffiliateProductMutation = useMutation({
    mutationFn: async (productId) => {
      return apiRequest('POST', '/api/affiliate/products', { productId });
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/affiliate/products'] });
    }
  });
  
  // Render referral code, affiliate stats, and products to promote
}
6. Tích hợp thanh toán
Hệ thống sử dụng phương thức thanh toán khi nhận hàng (COD) và chuẩn bị sẵn hạ tầng để tích hợp các phương thức thanh toán trực tuyến:

// Xử lý đơn hàng trong routes.ts
app.post("/api/orders", authMiddleware, async (req, res) => {
  try {
    const { items, shippingAddress, paymentMethod } = req.body;
    
    // Validation
    if (!items || !items.length || !shippingAddress || !paymentMethod) {
      return res.status(400).json({ message: "Thiếu thông tin đơn hàng" });
    }
    
    // Calculate totals
    let subtotal = 0;
    let shipping = 30000; // Fixed shipping cost
    
    // Create order
    const orderData = {
      userId: req.user!.id,
      shippingAddress,
      paymentMethod,
      status: ORDER_STATUS.PROCESSING,
      subtotal,
      shipping,
      totalAmount: subtotal + shipping
    };
    
    const order = await storage.createOrder(orderData);
    
    // Create order items
    for (const item of items) {
      // Add item to order
      // Update product inventory
    }
    
    // Clear cart
    await storage.clearCart(cart.id);
    
    res.status(201).json(order);
  } catch (error) {
    res.status(400).json({ message: "Không thể tạo đơn hàng", error });
  }
});
7. Hệ thống tiếp thị liên kết và mã giới thiệu
Hệ thống hỗ trợ người bán tạo mã giới thiệu và tiếp thị sản phẩm từ các cửa hàng khác:

// Trong storage.ts
async getSellerByReferralCode(code: string): Promise<Seller | undefined> {
  const [seller] = await db
    .select()
    .from(sellers)
    .where(eq(sellers.referralCode, code));
  return seller;
}
// Trong routes.ts - Đăng ký người bán với mã giới thiệu
app.post("/api/sellers", authMiddleware, async (req, res) => {
  try {
    const { referralCode, ...sellerData } = req.body;
    
    // Kiểm tra mã giới thiệu
    let referrerId = null;
    if (referralCode) {
      const referrer = await storage.getSellerByReferralCode(referralCode);
      if (referrer) {
        referrerId = referrer.id;
      }
    }
    
    // Tạo hồ sơ người bán
    const seller = await storage.createSeller({
      ...sellerData,
      userId: req.user!.id,
      referrerId
    });
    
    // Tạo ví điện tử cho người bán
    await storage.createWallet({
      userId: req.user!.id,
      balance: 0
    });
    
    res.status(201).json(seller);
  } catch (error) {
    res.status(400).json({ message: "Không thể đăng ký người bán", error });
  }
});
Phần II: Hệ thống quản trị (Admin)
1. Kiến trúc hậu đài
Hệ thống admin được thiết kế tách biệt khỏi phần người dùng chính, với URL riêng: https://tik-tok-admin-ngahunglchp.replit.app

1.1. Xác thực admin
File server/admin-api.ts xử lý xác thực và cung cấp API dành riêng cho admin:

// Xác thực JWT cho admin
const verifyAdminJWT = (req: Request, res: Response, next: NextFunction) => {
  const token = req.headers.authorization?.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ message: "Không có token xác thực" });
  }
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET!);
    if (decoded.role !== 'admin') {
      return res.status(403).json({ message: "Không có quyền truy cập" });
    }
    
    req.adminUser = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ message: "Token không hợp lệ hoặc đã hết hạn" });
  }
};
// API đăng nhập admin
router.post("/admin/login", async (req, res) => {
  try {
    const { username, password } = req.body;
    
    // Kiểm tra tài khoản admin
    const user = await storage.getUserByUsername(username);
    
    if (!user || user.role !== USER_ROLES.ADMIN || !(await comparePasswords(password, user.password))) {
      return res.status(401).json({ message: "Thông tin đăng nhập không hợp lệ" });
    }
    
    // Tạo token JWT
    const token = jwt.sign(
      { id: user.id, username: user.username, role: user.role },
      process.env.JWT_SECRET!,
      { expiresIn: '7d' }
    );
    
    res.json({ token, user: { id: user.id, username: user.username, role: user.role } });
  } catch (error) {
    res.status(500).json({ message: "Lỗi đăng nhập", error });
  }
});
1.2. Giao diện admin
Giao diện admin được xây dựng với layout chuyên biệt và dashboard tổng quan:

// components/admin/AdminLayout.tsx
export default function AdminLayout({ children }: { children: React.ReactNode }) {
  const { data: user } = useQuery({
    queryKey: ['/api/admin/user'],
    queryFn: getQueryFn()
  });
  
  if (!user) {
    return <AdminLogin />;
  }
  
  return (
    <div className="flex h-screen">
      <AdminSidebar />
      <div className="flex-1 overflow-auto">
        <AdminHeader />
        <main className="p-6">{children}</main>
      </div>
    </div>
  );
}
// pages/admin/dashboard.tsx
function AdminDashboardPage() {
  const { data: stats } = useQuery({
    queryKey: ['/api/admin/statistics/dashboard'],
    queryFn: getQueryFn()
  });
  
  const { data: recentOrders } = useQuery({
    queryKey: ['/api/admin/orders/recent'],
    queryFn: getQueryFn()
  });
  
  // Render dashboard with stats and recent activity
}
2. Quản lý người dùng và người bán
2.1. Xác minh người bán
Trang xác minh người bán (admin/seller-approval.tsx) liệt kê các người bán đang chờ phê duyệt:

function SellerApprovalPage() {
  const { data: pendingSellers } = useQuery({
    queryKey: ['/api/admin/sellers/pending'],
    queryFn: getQueryFn()
  });
  
  const verifySeller = useMutation({
    mutationFn: async (id) => {
      return apiRequest('PATCH', `/api/admin/sellers/${id}/verify`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/sellers/pending'] });
      toast({ title: "Xác minh người bán thành công" });
    }
  });
  
  const assignReferralCode = useMutation({
    mutationFn: async ({ id, code }) => {
      return apiRequest('PATCH', `/api/admin/sellers/${id}/assign-referral`, { code });
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/sellers/pending'] });
      toast({ title: "Đã cấp mã giới thiệu" });
    }
  });
  
  // Render list of pending sellers with verify and assign referral actions
}
2.2. Quản lý tài khoản
Trang quản lý tài khoản (admin/users.tsx) liệt kê và cho phép quản lý tất cả người dùng:

function UserManagementPage() {
  const { data: users } = useQuery({
    queryKey: ['/api/admin/users'],
    queryFn: getQueryFn()
  });
  
  // Status filter state
  const [roleFilter, setRoleFilter] = useState('all');
  
  // Filtered users
  const filteredUsers = users?.filter(user => {
    if (roleFilter === 'all') return true;
    return user.role === roleFilter;
  });
  
  // Actions
  const blockUser = useMutation({
    mutationFn: async (id) => {
      return apiRequest('PATCH', `/api/admin/users/${id}/block`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/users'] });
    }
  });
  
  // Render user list with filters and actions
}
3. Quản lý sản phẩm
3.1. Phê duyệt sản phẩm
Trang phê duyệt sản phẩm (admin/product-approval.tsx) liệt kê các sản phẩm đang chờ phê duyệt:

function ProductApprovalPage() {
  const { data: pendingProducts } = useQuery({
    queryKey: ['/api/admin/products/pending'],
    queryFn: getQueryFn()
  });
  
  const approveProduct = useMutation({
    mutationFn: async (id) => {
      return apiRequest('PATCH', `/api/admin/products/${id}/approve`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/products/pending'] });
      toast({ title: "Đã phê duyệt sản phẩm" });
    }
  });
  
  const rejectProduct = useMutation({
    mutationFn: async (id) => {
      return apiRequest('PATCH', `/api/admin/products/${id}/reject`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/products/pending'] });
      toast({ title: "Đã từ chối sản phẩm" });
    }
  });
  
  // Render list of pending products with approve and reject actions
}
3.2. Quản lý danh mục
Trang quản lý danh mục (admin/categories.tsx) cho phép thêm, sửa, xóa danh mục:

function CategoryManagementPage() {
  const { data: categories } = useQuery({
    queryKey: ['/api/categories'],
    queryFn: getQueryFn()
  });
  
  const addCategory = useMutation({
    mutationFn: async (data) => {
      return apiRequest('POST', '/api/categories', data);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/categories'] });
      toast({ title: "Đã thêm danh mục" });
    }
  });
  
  const updateCategory = useMutation({
    mutationFn: async ({ id, data }) => {
      return apiRequest('PATCH', `/api/categories/${id}`, data);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/categories'] });
      toast({ title: "Đã cập nhật danh mục" });
    }
  });
  
  const deleteCategory = useMutation({
    mutationFn: async (id) => {
      return apiRequest('DELETE', `/api/categories/${id}`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/categories'] });
      toast({ title: "Đã xóa danh mục" });
    }
  });
  
  // Render category list with add, edit, and delete actions
}
3.3. Cửa hàng mẫu
Trang quản lý cửa hàng mẫu (admin/sample-store.tsx) cho phép tạo sản phẩm mẫu:

function SampleStorePage() {
  const { data: categories } = useQuery({
    queryKey: ['/api/categories'],
    queryFn: getQueryFn()
  });
  
  const { data: sampleProducts } = useQuery({
    queryKey: ['/api/admin/products/sample-store'],
    queryFn: getQueryFn()
  });
  
  const createSampleProduct = useMutation({
    mutationFn: async (data) => {
      const formData = new FormData();
      // Append product data and images
      return apiRequest('POST', '/api/admin/products/sample-store', formData, true);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/products/sample-store'] });
      toast({ title: "Đã tạo sản phẩm mẫu" });
    }
  });
  
  // Render sample product form and list
}
4. Quản lý đơn hàng
Trang quản lý đơn hàng (admin/orders.tsx) hiển thị và cho phép cập nhật trạng thái đơn hàng:

function OrderManagementPage() {
  const { data: orders } = useQuery({
    queryKey: ['/api/admin/orders'],
    queryFn: getQueryFn()
  });
  
  // Status filter state
  const [statusFilter, setStatusFilter] = useState('all');
  
  // Filtered orders
  const filteredOrders = orders?.filter(order => {
    if (statusFilter === 'all') return true;
    return order.status === statusFilter;
  });
  
  // Update order status
  const updateOrderStatus = useMutation({
    mutationFn: async ({ id, status }) => {
      return apiRequest('PATCH', `/api/admin/orders/${id}/status`, { status });
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/orders'] });
      toast({ title: "Đã cập nhật trạng thái đơn hàng" });
    }
  });
  
  // Render orders with filters and update actions
}
5. Quản lý giao dịch tài chính
5.1. Phê duyệt rút tiền
Trang quản lý rút tiền (admin/withdrawals.tsx) liệt kê và phê duyệt/từ chối yêu cầu rút tiền:

function WithdrawalManagementPage() {
  const { data: pendingWithdrawals } = useQuery({
    queryKey: ['/api/admin/withdrawals/pending'],
    queryFn: getQueryFn()
  });
  
  const approveWithdrawal = useMutation({
    mutationFn: async (id) => {
      return apiRequest('PATCH', `/api/admin/withdrawals/${id}/approve`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/withdrawals/pending'] });
      toast({ title: "Đã phê duyệt yêu cầu rút tiền" });
    }
  });
  
  const rejectWithdrawal = useMutation({
    mutationFn: async (id) => {
      return apiRequest('PATCH', `/api/admin/withdrawals/${id}/reject`);
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['/api/admin/withdrawals/pending'] });
      toast({ title: "Đã từ chối yêu cầu rút tiền" });
    }
  });
  
  // Render withdrawal requests with approve and reject actions
}
5.2. Thống kê doanh thu
Trang thống kê doanh thu (admin/revenue.tsx) hiển thị biểu đồ và số liệu doanh thu:

function RevenueStatisticsPage() {
  const { data: dailyRevenue } = useQuery({
    queryKey: ['/api/admin/statistics/revenue/daily'],
    queryFn: getQueryFn()
  });
  
  const { data: monthlyRevenue } = useQuery({
    queryKey: ['/api/admin/statistics/revenue/monthly'],
    queryFn: getQueryFn()
  });
  
  const { data: categoryRevenue } = useQuery({
    queryKey: ['/api/admin/statistics/revenue/by-category'],
    queryFn: getQueryFn()
  });
  
  // Render charts and statistics
  return (
    <div>
      <h1>Thống kê doanh thu</h1>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <StatCard title="Tổng doanh thu" value={formatCurrency(totalRevenue)} />
        <StatCard title="Doanh thu tháng này" value={formatCurrency(monthlyTotal)} />
        <StatCard title="Doanh thu hôm nay" value={formatCurrency(dailyTotal)} />
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card>
          <CardHeader>
            <CardTitle>Doanh thu theo ngày</CardTitle>
          </CardHeader>
          <CardContent className="h-80">
            <BarChart data={dailyRevenue} />
          </CardContent>
        </Card>
        
        <Card>
          <CardHeader>
            <CardTitle>Doanh thu theo danh mục</CardTitle>
          </CardHeader>
          <CardContent className="h-80">
            <PieChart data={categoryRevenue} />
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
6. Dữ liệu thống kê và báo cáo
Trang Dashboard chính hiển thị tổng quan về hoạt động của sàn:

function AdminDashboardPage() {
  const { data: stats } = useQuery({
    queryKey: ['/api/admin/statistics/dashboard'],
    queryFn: getQueryFn()
  });
  
  const { data: recentOrders } = useQuery({
    queryKey: ['/api/admin/orders/recent'],
    queryFn: getQueryFn()
  });
  
  const { data: recentProducts } = useQuery({
    queryKey: ['/api/admin/products/recent'],
    queryFn: getQueryFn()
  });
  
  const { data: recentSellers } = useQuery({
    queryKey: ['/api/admin/sellers/recent'],
    queryFn: getQueryFn()
  });
  
  return (
    <div>
      <h1>Tổng quan hệ thống</h1>
      
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <StatCard 
          title="Người dùng" 
          value={stats?.totalUsers} 
          description={`+ ${stats?.newUsersToday} hôm nay`}
          icon={<Users className="h-8 w-8" />}
        />
        
        <StatCard 
          title="Đơn hàng" 
          value={stats?.totalOrders} 
          description={`+ ${stats?.ordersToday} hôm nay`}
          icon={<ShoppingCart className="h-8 w-8" />}
        />
        
        <StatCard 
          title="Sản phẩm" 
          value={stats?.totalProducts} 
          description={`${stats?.pendingProducts} chờ duyệt`}
          icon={<Package className="h-8 w-8" />}
        />
        
        <StatCard 
          title="Doanh thu" 
          value={formatCurrency(stats?.totalRevenue)} 
          description={`+ ${formatCurrency(stats?.revenueToday)} hôm nay`}
          icon={<DollarSign className="h-8 w-8" />}
        />
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
        <ApprovalWidget 
          productCount={stats?.pendingProducts} 
          sellerCount={stats?.pendingSellers} 
        />
        
        <WithdrawalWidget 
          count={stats?.pendingWithdrawals} 
          amount={stats?.pendingWithdrawalAmount} 
        />
      </div>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <RecentOrdersTable orders={recentOrders} />
        <RecentProductsTable products={recentProducts} />
      </div>
    </div>
  );
}
7. Tạo dữ liệu mẫu
Để bổ sung và làm phong phú thêm dữ liệu cho sàn thương mại, hệ thống hỗ trợ tạo dữ liệu mẫu:

7.1. API tạo dữ liệu mẫu
// Trong routes.ts
app.post("/api/admin/products/sample-store", adminMiddleware, async (req: Request, res: Response) => {
  try {
    const productData = {
      ...req.body,
      status: PRODUCT_STATUS.APPROVED,
      isSampleStore: true
    };
    
    const product = await storage.createProduct(productData);
    res.status(201).json(product);
  } catch (error) {
    res.status(400).json({ message: "Không thể tạo sản phẩm mẫu", error });
  }
});
7.2. Công cụ dòng lệnh tạo dữ liệu mẫu
File utils/sample-products-creator.js tự động tạo sản phẩm mẫu:

// Các thông tin mẫu
const CATEGORIES = {
  THOI_TRANG: 1,
  DIEN_TU: 2,
  NOI_THAT: 3,
  // ...
};
const SAMPLE_PRODUCTS = {
  [CATEGORIES.THOI_TRANG]: [
    {
      name: "Áo Thun Unisex TikTok Trends",
      description: "Áo thun unisex chất liệu cotton mềm mại, thoáng mát...",
      price: 199000,
      discountedPrice: 149000,
      stock: 100,
      isFeatured: true
    },
    // Các sản phẩm mẫu khác...
  ],
  // Các danh mục khác...
};
// Hàm tạo sản phẩm mẫu
async function createSampleProducts() {
  try {
    const allCategories = Object.keys(CATEGORIES);
    let createdProducts = 0;
    
    for (const categoryKey of allCategories) {
      const categoryId = CATEGORIES[categoryKey];
      const products = SAMPLE_PRODUCTS[categoryId];
      const images = PRODUCT_IMAGES[categoryId];
      
      for (let i = 0; i < products.length; i++) {
        const product = products[i];
        const imageUrl = images[i % images.length];
        
        // Thêm thông tin sellerId và markSampleStore
        const productData = {
          ...product,
          sellerId: 1,
          categoryId,
          status: 'approved',
          isSampleStore: true
        };
        
        // Tạo sản phẩm
        const createdProduct = await callAPI('/api/admin/products/sample-store', 'POST', productData, JWT_TOKEN);
        
        // Thêm hình ảnh cho sản phẩm
        const imageData = {
          productId: createdProduct.id,
          url: imageUrl,
          isMain: true
        };
        
        await callAPI(`/api/products/${createdProduct.id}/images`, 'POST', imageData, JWT_TOKEN);
        
        createdProducts++;
      }
    }
    
    console.log(`✅ Đã tạo thành công ${createdProducts} sản phẩm mẫu!`);
  } catch (error) {
    console.error('Lỗi khi tạo sản phẩm mẫu:', error);
  }
}
Triển khai và vận hành
Cấu hình triển khai
Dự án được triển khai trên nền tảng Replit với các cấu hình sau:

Cấu hình Vite: Trong vite.config.ts để xử lý bundling và development
Replit Toml: Định nghĩa cấu hình môi trường và deployment
Tên miền tùy chỉnh: Cấu hình trong .replit.domain để sử dụng tên miền tiktoksshopp.com
Quản lý môi trường
Biến môi trường được quản lý thông qua Replit Secrets:

DATABASE_URL: Kết nối PostgreSQL
JWT_SECRET: Khóa bảo mật cho token
SESSION_SECRET: Khóa bảo mật cho phiên đăng nhập
Bảo mật hệ thống
Xác thực và phân quyền
JWT: Sử dụng cho xác thực API hậu đài
Session: Sử dụng cho xác thực người dùng và người bán
Middleware: Triển khai authMiddleware, sellerMiddleware, và adminMiddleware để kiểm soát quyền truy cập
Bảo mật dữ liệu
Mã hóa mật khẩu: Sử dụng scrypt để băm mật khẩu
HTTPS: Bảo mật giao tiếp qua HTTPS
Input validation: Sử dụng Zod để xác thực dữ liệu đầu vào
Prepared statements: Sử dụng Drizzle ORM để tránh SQL injection
Logs và giám sát
Console logs: Ghi lại các sự kiện quan trọng
Error handling: Xử lý lỗi đầy đủ với thông báo phù hợp
Dashboard theo dõi: Hiển thị metrics về hoạt động của hệ thống
