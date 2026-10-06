import 'package:flutter/material.dart';

void main() {
  runApp(const CampusCanteenApp());
}

// ======================================================
// APP
// ======================================================

class CampusCanteenApp extends StatelessWidget {
  const CampusCanteenApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: "Campus Canteen",
      theme: ThemeData(
        primarySwatch: Colors.green,
        scaffoldBackgroundColor: const Color(0xFFF5F9F5),
        appBarTheme: const AppBarTheme(
          backgroundColor: Color(0xFF2E7D32),
          foregroundColor: Colors.white,
        ),
      ),
      home: const LoginPage(),
    );
  }
}

// ======================================================
// FOOD MODEL
// ======================================================

class Food {
  String name;
  double price;

  Food({
    required this.name,
    required this.price,
  });
}

// ======================================================
// GLOBAL DATA
// ======================================================

List<Food> foodList = [
  Food(name: "Masala Dosa", price: 50),
  Food(name: "Idly", price: 30),
  Food(name: "Veg Fried Rice", price: 80),
  Food(name: "Veg Noodles", price: 70),
  Food(name: "Samosa", price: 20),
  Food(name: "Veg Sandwich", price: 60),
];

List<Food> drinkList = [
  Food(name: "Fresh Lime Juice", price: 30),
  Food(name: "Mango Juice", price: 40),
  Food(name: "Orange Juice", price: 40),
  Food(name: "Milkshake", price: 60),
];

// Cart
Map<Food, int> cart = {};

// Order history
List<Map<String, dynamic>> orderHistory = [];

// ======================================================
// LOGIN PAGE
// ======================================================

class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  final TextEditingController emailController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();

  void login() {
    String email = emailController.text.trim();

    // Check empty Gmail
    if (email.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text("Please enter your Gmail address"),
          backgroundColor: Colors.red,
        ),
      );
      return;
    }

    // Check Gmail format
    if (!email.toLowerCase().endsWith("@gmail.com")) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text("Please enter a valid Gmail address"),
          backgroundColor: Colors.red,
        ),
      );
      return;
    }

    // Login accepted
    Navigator.pushReplacement(
      context,
      MaterialPageRoute(
        builder: (context) => HomePage(email: email),
      ),
    );
  }

  @override
  void dispose() {
    emailController.dispose();
    passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: const Color(0xFFE8F5E9),
      body: Center(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(25),
          child: Card(
            elevation: 8,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
            child: Padding(
              padding: const EdgeInsets.all(25),
              child: Column(
                children: [
                  const Icon(
                    Icons.restaurant,
                    size: 70,
                    color: Color(0xFF2E7D32),
                  ),

                  const SizedBox(height: 15),

                  const Text(
                    "Campus Canteen",
                    style: TextStyle(
                      fontSize: 28,
                      fontWeight: FontWeight.bold,
                      color: Color(0xFF2E7D32),
                    ),
                  ),

                  const SizedBox(height: 8),

                  const Text(
                    "Login to continue",
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.grey,
                    ),
                  ),

                  const SizedBox(height: 30),

                  // Gmail
                  TextField(
                    controller: emailController,
                    keyboardType: TextInputType.emailAddress,
                    decoration: InputDecoration(
                      labelText: "Gmail",
                      hintText: "example@gmail.com",
                      prefixIcon: const Icon(Icons.email),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                    ),
                  ),

                  const SizedBox(height: 15),

                  // Password
                  TextField(
                    controller: passwordController,
                    obscureText: true,
                    decoration: InputDecoration(
                      labelText: "Password",
                      prefixIcon: const Icon(Icons.lock),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                    ),
                  ),

                  const SizedBox(height: 25),

                  // Login button
                  SizedBox(
                    width: double.infinity,
                    height: 50,
                    child: ElevatedButton(
                      onPressed: login,
                      style: ElevatedButton.styleFrom(
                        backgroundColor: const Color(0xFF2E7D32),
                        foregroundColor: Colors.white,
                        shape: RoundedRectangleBorder(
                          borderRadius: BorderRadius.circular(12),
                        ),
                      ),
                      child: const Text(
                        "LOGIN",
                        style: TextStyle(
                          fontSize: 17,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                  ),

                  const SizedBox(height: 15),

                  // Google
                  SizedBox(
                    width: double.infinity,
                    height: 48,
                    child: OutlinedButton.icon(
                      onPressed: () {
                        emailController.text = "student@gmail.com";
                        login();
                      },
                      icon: const Icon(
                        Icons.g_mobiledata,
                        size: 30,
                      ),
                      label: const Text(
                        "Continue with Google",
                      ),
                    ),
                  ),

                  const SizedBox(height: 10),

                  // Facebook
                  SizedBox(
                    width: double.infinity,
                    height: 48,
                    child: OutlinedButton.icon(
                      onPressed: () {
                        emailController.text = "student@gmail.com";
                        login();
                      },
                      icon: const Icon(Icons.facebook),
                      label: const Text(
                        "Continue with Facebook",
                      ),
                    ),
                  ),

                  const SizedBox(height: 20),

                  // Sign up
                  TextButton(
                    onPressed: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(
                          builder: (context) => const SignUpPage(),
                        ),
                      );
                    },
                    child: const Text(
                      "Don't have an account? Sign Up",
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}

// ======================================================
// SIGN UP PAGE
// ======================================================

class SignUpPage extends StatefulWidget {
  const SignUpPage({super.key});

  @override
  State<SignUpPage> createState() => _SignUpPageState();
}

class _SignUpPageState extends State<SignUpPage> {
  final TextEditingController nameController = TextEditingController();
  final TextEditingController emailController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();

  void signUp() {
    String name = nameController.text.trim();
    String email = emailController.text.trim();
    String password = passwordController.text.trim();

    if (name.isEmpty || email.isEmpty || password.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text("Please fill all the fields"),
          backgroundColor: Colors.red,
        ),
      );
      return;
    }

    if (!email.toLowerCase().endsWith("@gmail.com")) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text("Please use a Gmail address"),
          backgroundColor: Colors.red,
        ),
      );
      return;
    }

    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text("Account created successfully!"),
        backgroundColor: Colors.green,
      ),
    );

    Future.delayed(const Duration(seconds: 1), () {
      if (!mounted) return;

      Navigator.pop(context);
    });
  }

  @override
  void dispose() {
    nameController.dispose();
    emailController.dispose();
    passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Sign Up"),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(25),
        child: Column(
          children: [
            const Icon(
              Icons.person_add,
              size: 80,
              color: Color(0xFF2E7D32),
            ),

            const SizedBox(height: 20),

            TextField(
              controller: nameController,
              decoration: InputDecoration(
                labelText: "Full Name",
                prefixIcon: const Icon(Icons.person),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(12),
                ),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: emailController,
              keyboardType: TextInputType.emailAddress,
              decoration: InputDecoration(
                labelText: "Gmail",
                hintText: "example@gmail.com",
                prefixIcon: const Icon(Icons.email),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(12),
                ),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: passwordController,
              obscureText: true,
              decoration: InputDecoration(
                labelText: "Password",
                prefixIcon: const Icon(Icons.lock),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(12),
                ),
              ),
            ),

            const SizedBox(height: 25),

            SizedBox(
              width: double.infinity,
              height: 50,
              child: ElevatedButton(
                onPressed: signUp,
                style: ElevatedButton.styleFrom(
                  backgroundColor: const Color(0xFF2E7D32),
                  foregroundColor: Colors.white,
                ),
                child: const Text(
                  "CREATE ACCOUNT",
                  style: TextStyle(
                    fontSize: 16,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

// ======================================================
// HOME PAGE
// ======================================================

class HomePage extends StatelessWidget {
  final String email;

  const HomePage({
    super.key,
    required this.email,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Campus Canteen"),

        actions: [
          IconButton(
            icon: const Icon(Icons.person),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => ProfilePage(email: email),
                ),
              );
            },
          ),
        ],
      ),

      body: SingleChildScrollView(
        padding: const EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              "Welcome! 👋",
              style: const TextStyle(
                fontSize: 28,
                fontWeight: FontWeight.bold,
                color: Color(0xFF2E7D32),
              ),
            ),

            const SizedBox(height: 5),

            Text(
              email,
              style: const TextStyle(
                color: Colors.grey,
                fontSize: 15,
              ),
            ),

            const SizedBox(height: 25),

            const Text(
              "What would you like?",
              style: TextStyle(
                fontSize: 20,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 20),

            // Food
            _menuCard(
              context,
              title: "Food Menu",
              subtitle: "Dosa, Idly, Rice, Snacks & more",
              icon: Icons.restaurant_menu,
              color: const Color(0xFF2E7D32),
              page: const FoodPage(),
            ),

            const SizedBox(height: 15),

            // Drinks
            _menuCard(
              context,
              title: "Drinks",
              subtitle: "Juices & Milkshakes",
              icon: Icons.local_drink,
              color: Colors.orange,
              page: const DrinksPage(),
            ),

            const SizedBox(height: 15),

            // Cart
            _menuCard(
              context,
              title: "My Cart",
              subtitle: "View selected food and drinks",
              icon: Icons.shopping_cart,
              color: Colors.blue,
              page: const CartPage(),
            ),

            const SizedBox(height: 15),

            // Orders
            _menuCard(
              context,
              title: "Total Orders",
              subtitle: "View your previous orders",
              icon: Icons.receipt_long,
              color: Colors.purple,
              page: const TotalOrdersPage(),
            ),
          ],
        ),
      ),
    );
  }

  Widget _menuCard(
    BuildContext context, {
    required String title,
    required String subtitle,
    required IconData icon,
    required Color color,
    required Widget page,
  }) {
    return Card(
      elevation: 4,
      child: ListTile(
        contentPadding: const EdgeInsets.all(15),
        leading: CircleAvatar(
          radius: 28,
          backgroundColor: color.withOpacity(0.15),
          child: Icon(
            icon,
            color: color,
            size: 30,
          ),
        ),
        title: Text(
          title,
          style: const TextStyle(
            fontWeight: FontWeight.bold,
            fontSize: 18,
          ),
        ),
        subtitle: Padding(
          padding: const EdgeInsets.only(top: 5),
          child: Text(subtitle),
        ),
        trailing: const Icon(Icons.arrow_forward_ios),
        onTap: () {
          Navigator.push(
            context,
            MaterialPageRoute(
              builder: (context) => page,
            ),
          );
        },
      ),
    );
  }
}

// ======================================================
// FOOD PAGE
// ======================================================

class FoodPage extends StatefulWidget {
  const FoodPage({super.key});

  @override
  State<FoodPage> createState() => _FoodPageState();
}

class _FoodPageState extends State<FoodPage> {
  void showFoodDialog({
    Food? food,
    required int index,
  }) {
    final nameController = TextEditingController(
      text: food?.name ?? "",
    );

    final priceController = TextEditingController(
      text: food != null ? food.price.toString() : "",
    );

    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text(
            food == null ? "Add Food" : "Edit Food",
          ),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              TextField(
                controller: nameController,
                decoration: const InputDecoration(
                  labelText: "Food Name",
                ),
              ),

              const SizedBox(height: 10),

              TextField(
                controller: priceController,
                keyboardType: TextInputType.number,
                decoration: const InputDecoration(
                  labelText: "Price",
                ),
              ),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text("Cancel"),
            ),

            ElevatedButton(
              onPressed: () {
                String name = nameController.text.trim();
                double? price = double.tryParse(
                  priceController.text.trim(),
                );

                if (name.isEmpty || price == null) {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(
                      content: Text("Enter valid food details"),
                    ),
                  );
                  return;
                }

                setState(() {
                  if (food == null) {
                    foodList.add(
                      Food(
                        name: name,
                        price: price,
                      ),
                    );
                  } else {
                    food.name = name;
                    food.price = price;
                  }
                });

                Navigator.pop(context);
              },
              child: const Text("Save"),
            ),
          ],
        );
      },
    );
  }

  void deleteFood(int index) {
    setState(() {
      Food food = foodList[index];

      cart.remove(food);
      foodList.removeAt(index);
    });
  }

  void addToCart(Food food) {
    setState(() {
      cart[food] = (cart[food] ?? 0) + 1;
    });

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text("${food.name} added to cart"),
        duration: const Duration(seconds: 1),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Food Menu"),
        actions: [
          IconButton(
            icon: const Icon(Icons.shopping_cart),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => const CartPage(),
                ),
              );
            },
          ),
        ],
      ),

      floatingActionButton: FloatingActionButton(
        backgroundColor: const Color(0xFF2E7D32),
        onPressed: () {
          showFoodDialog(
            index: -1,
          );
        },
        child: const Icon(
          Icons.add,
          color: Colors.white,
        ),
      ),

      body: foodList.isEmpty
          ? const Center(
              child: Text(
                "No food items available",
                style: TextStyle(fontSize: 18),
              ),
            )
          : ListView.builder(
              padding: const EdgeInsets.all(15),
              itemCount: foodList.length,
              itemBuilder: (context, index) {
                final food = foodList[index];

                return Card(
                  elevation: 3,
                  margin: const EdgeInsets.only(bottom: 12),
                  child: ListTile(
                    contentPadding: const EdgeInsets.all(12),

                    leading: CircleAvatar(
                      backgroundColor: const Color(0xFFE8F5E9),
                      child: const Icon(
                        Icons.restaurant,
                        color: Color(0xFF2E7D32),
                      ),
                    ),

                    title: Text(
                      food.name,
                      style: const TextStyle(
                        fontWeight: FontWeight.bold,
                      ),
                    ),

                    subtitle: Text(
                      "₹${food.price.toStringAsFixed(2)}",
                      style: const TextStyle(
                        color: Color(0xFF2E7D32),
                        fontWeight: FontWeight.bold,
                      ),
                    ),

                    trailing: Wrap(
                      spacing: 2,
                      children: [
                        IconButton(
                          icon: const Icon(
                            Icons.add_shopping_cart,
                            color: Colors.green,
                          ),
                          onPressed: () {
                            addToCart(food);
                          },
                        ),

                        IconButton(
                          icon: const Icon(
                            Icons.edit,
                            color: Colors.blue,
                          ),
                          onPressed: () {
                            showFoodDialog(
                              food: food,
                              index: index,
                            );
                          },
                        ),

                        IconButton(
                          icon: const Icon(
                            Icons.delete,
                            color: Colors.red,
                          ),
                          onPressed: () {
                            deleteFood(index);
                          },
                        ),
                      ],
                    ),
                  ),
                );
              },
            ),
    );
  }
}

// ======================================================
// DRINKS PAGE
// ======================================================

class DrinksPage extends StatefulWidget {
  const DrinksPage({super.key});

  @override
  State<DrinksPage> createState() => _DrinksPageState();
}

class _DrinksPageState extends State<DrinksPage> {
  void showDrinkDialog({
    Food? drink,
  }) {
    final nameController = TextEditingController(
      text: drink?.name ?? "",
    );

    final priceController = TextEditingController(
      text: drink != null ? drink.price.toString() : "",
    );

    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text(
            drink == null ? "Add Drink" : "Edit Drink",
          ),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              TextField(
                controller: nameController,
                decoration: const InputDecoration(
                  labelText: "Drink Name",
                ),
              ),

              const SizedBox(height: 10),

              TextField(
                controller: priceController,
                keyboardType: TextInputType.number,
                decoration: const InputDecoration(
                  labelText: "Price",
                ),
              ),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text("Cancel"),
            ),

            ElevatedButton(
              onPressed: () {
                String name = nameController.text.trim();

                double? price = double.tryParse(
                  priceController.text.trim(),
                );

                if (name.isEmpty || price == null) {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(
                      content: Text("Enter valid drink details"),
                    ),
                  );
                  return;
                }

                setState(() {
                  if (drink == null) {
                    drinkList.add(
                      Food(
                        name: name,
                        price: price,
                      ),
                    );
                  } else {
                    drink.name = name;
                    drink.price = price;
                  }
                });

                Navigator.pop(context);
              },
              child: const Text("Save"),
            ),
          ],
        );
      },
    );
  }

  void deleteDrink(int index) {
    setState(() {
      Food drink = drinkList[index];

      cart.remove(drink);
      drinkList.removeAt(index);
    });
  }

  void addToCart(Food drink) {
    setState(() {
      cart[drink] = (cart[drink] ?? 0) + 1;
    });

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text("${drink.name} added to cart"),
        duration: const Duration(seconds: 1),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Drinks"),
        actions: [
          IconButton(
            icon: const Icon(Icons.shopping_cart),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => const CartPage(),
                ),
              );
            },
          ),
        ],
      ),

      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.orange,
        onPressed: () {
          showDrinkDialog();
        },
        child: const Icon(
          Icons.add,
          color: Colors.white,
        ),
      ),

      body: drinkList.isEmpty
          ? const Center(
              child: Text(
                "No drinks available",
                style: TextStyle(fontSize: 18),
              ),
            )
          : ListView.builder(
              padding: const EdgeInsets.all(15),
              itemCount: drinkList.length,
              itemBuilder: (context, index) {
                final drink = drinkList[index];

                return Card(
                  elevation: 3,
                  margin: const EdgeInsets.only(bottom: 12),
                  child: ListTile(
                    contentPadding: const EdgeInsets.all(12),

                    leading: CircleAvatar(
                      backgroundColor: const Color(0xFFFFF3E0),
                      child: const Icon(
                        Icons.local_drink,
                        color: Colors.orange,
                      ),
                    ),

                    title: Text(
                      drink.name,
                      style: const TextStyle(
                        fontWeight: FontWeight.bold,
                      ),
                    ),

                    subtitle: Text(
                      "₹${drink.price.toStringAsFixed(2)}",
                      style: const TextStyle(
                        color: Colors.orange,
                        fontWeight: FontWeight.bold,
                      ),
                    ),

                    trailing: Wrap(
                      spacing: 2,
                      children: [
                        IconButton(
                          icon: const Icon(
                            Icons.add_shopping_cart,
                            color: Colors.green,
                          ),
                          onPressed: () {
                            addToCart(drink);
                          },
                        ),

                        IconButton(
                          icon: const Icon(
                            Icons.edit,
                            color: Colors.blue,
                          ),
                          onPressed: () {
                            showDrinkDialog(
                              drink: drink,
                            );
                          },
                        ),

                        IconButton(
                          icon: const Icon(
                            Icons.delete,
                            color: Colors.red,
                          ),
                          onPressed: () {
                            deleteDrink(index);
                          },
                        ),
                      ],
                    ),
                  ),
                );
              },
            ),
    );
  }
}

// ======================================================
// CART PAGE
// ======================================================

class CartPage extends StatefulWidget {
  const CartPage({super.key});

  @override
  State<CartPage> createState() => _CartPageState();
}

class _CartPageState extends State<CartPage> {
  double get total {
    double sum = 0;

    cart.forEach((food, quantity) {
      sum += food.price * quantity;
    });

    return sum;
  }

  void increase(Food food) {
    setState(() {
      cart[food] = (cart[food] ?? 0) + 1;
    });
  }

  void decrease(Food food) {
    setState(() {
      if (cart[food] != null && cart[food]! > 1) {
        cart[food] = cart[food]! - 1;
      } else {
        cart.remove(food);
      }
    });
  }

  void placeOrder() {
    if (cart.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text("Your cart is empty"),
          backgroundColor: Colors.red,
        ),
      );
      return;
    }

    double orderTotal = total;

    orderHistory.add({
      "date": DateTime.now(),
      "total": orderTotal,
      "items": Map<Food, int>.from(cart),
    });

    setState(() {
      cart.clear();
    });

    Navigator.pushReplacement(
      context,
      MaterialPageRoute(
        builder: (context) => SuccessPage(
          total: orderTotal,
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("My Cart"),
      ),

      body: cart.isEmpty
          ? const Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    Icons.shopping_cart_outlined,
                    size: 80,
                    color: Colors.grey,
                  ),
                  SizedBox(height: 15),
                  Text(
                    "Your cart is empty",
                    style: TextStyle(
                      fontSize: 20,
                      color: Colors.grey,
                    ),
                  ),
                ],
              ),
            )
          : Column(
              children: [
                Expanded(
                  child: ListView(
                    padding: const EdgeInsets.all(15),
                    children: cart.keys.map((food) {
                      int quantity = cart[food]!;

                      return Card(
                        child: ListTile(
                          title: Text(
                            food.name,
                            style: const TextStyle(
                              fontWeight: FontWeight.bold,
                            ),
                          ),

                          subtitle: Text(
                            "₹${food.price.toStringAsFixed(2)} each",
                          ),

                          trailing: Row(
                            mainAxisSize: MainAxisSize.min,
                            children: [
                              IconButton(
                                icon: const Icon(
                                  Icons.remove_circle_outline,
                                ),
                                onPressed: () {
                                  decrease(food);
                                },
                              ),

                              Text(
                                "$quantity",
                                style: const TextStyle(
                                  fontSize: 18,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),

                              IconButton(
                                icon: const Icon(
                                  Icons.add_circle_outline,
                                  color: Colors.green,
                                ),
                                onPressed: () {
                                  increase(food);
                                },
                              ),
                            ],
                          ),
                        ),
                      );
                    }).toList(),
                  ),
                ),

                Container(
                  padding: const EdgeInsets.all(20),
                  decoration: const BoxDecoration(
                    color: Colors.white,
                    boxShadow: [
                      BoxShadow(
                        blurRadius: 5,
                        color: Colors.black12,
                      ),
                    ],
                  ),
                  child: Column(
                    children: [
                      Row(
                        mainAxisAlignment:
                            MainAxisAlignment.spaceBetween,
                        children: [
                          const Text(
                            "Total",
                            style: TextStyle(
                              fontSize: 20,
                              fontWeight: FontWeight.bold,
                            ),
                          ),

                          Text(
                            "₹${total.toStringAsFixed(2)}",
                            style: const TextStyle(
                              fontSize: 22,
                              fontWeight: FontWeight.bold,
                              color: Color(0xFF2E7D32),
                            ),
                          ),
                        ],
                      ),

                      const SizedBox(height: 15),

                      SizedBox(
                        width: double.infinity,
                        height: 50,
                        child: ElevatedButton(
                          onPressed: placeOrder,
                          style: ElevatedButton.styleFrom(
                            backgroundColor:
                                const Color(0xFF2E7D32),
                            foregroundColor: Colors.white,
                          ),
                          child: const Text(
                            "PLACE ORDER",
                            style: TextStyle(
                              fontWeight: FontWeight.bold,
                              fontSize: 16,
                            ),
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
    );
  }
}

// ======================================================
// SUCCESS PAGE
// ======================================================

class SuccessPage extends StatelessWidget {
  final double total;

  const SuccessPage({
    super.key,
    required this.total,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Order Successful"),
      ),

      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(30),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Icon(
                Icons.check_circle,
                color: Colors.green,
                size: 100,
              ),

              const SizedBox(height: 20),

              const Text(
                "Order Placed Successfully!",
                textAlign: TextAlign.center,
                style: TextStyle(
                  fontSize: 25,
                  fontWeight: FontWeight.bold,
                  color: Color(0xFF2E7D32),
                ),
              ),

              const SizedBox(height: 15),

              Text(
                "Total Amount: ₹${total.toStringAsFixed(2)}",
                style: const TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                ),
              ),

              const SizedBox(height: 30),

              SizedBox(
                width: double.infinity,
                height: 50,
                child: ElevatedButton(
                  onPressed: () {
                    Navigator.pop(context);
                  },
                  style: ElevatedButton.styleFrom(
                    backgroundColor: const Color(0xFF2E7D32),
                    foregroundColor: Colors.white,
                  ),
                  child: const Text("BACK TO HOME"),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// ======================================================
// TOTAL ORDERS PAGE
// ======================================================

class TotalOrdersPage extends StatefulWidget {
  const TotalOrdersPage({super.key});

  @override
  State<TotalOrdersPage> createState() => _TotalOrdersPageState();
}

class _TotalOrdersPageState extends State<TotalOrdersPage> {
  double get totalSpent {
    double sum = 0;

    for (var order in orderHistory) {
      sum += order["total"] as double;
    }

    return sum;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Total Orders"),
      ),

      body: orderHistory.isEmpty
          ? const Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    Icons.receipt_long,
                    size: 80,
                    color: Colors.grey,
                  ),
                  SizedBox(height: 15),
                  Text(
                    "No orders yet",
                    style: TextStyle(
                      fontSize: 20,
                      color: Colors.grey,
                    ),
                  ),
                ],
              ),
            )
          : Column(
              children: [
                Container(
                  width: double.infinity,
                  margin: const EdgeInsets.all(15),
                  padding: const EdgeInsets.all(20),
                  decoration: BoxDecoration(
                    color: const Color(0xFFE8F5E9),
                    borderRadius: BorderRadius.circular(15),
                  ),
                  child: Column(
                    children: [
                      const Text(
                        "Total Orders",
                        style: TextStyle(
                          fontSize: 17,
                          color: Colors.grey,
                        ),
                      ),

                      const SizedBox(height: 5),

                      Text(
                        "${orderHistory.length}",
                        style: const TextStyle(
                          fontSize: 32,
                          fontWeight: FontWeight.bold,
                          color: Color(0xFF2E7D32),
                        ),
                      ),

                      const SizedBox(height: 10),

                      const Text(
                        "Total Spent",
                        style: TextStyle(
                          fontSize: 17,
                          color: Colors.grey,
                        ),
                      ),

                      Text(
                        "₹${totalSpent.toStringAsFixed(2)}",
                        style: const TextStyle(
                          fontSize: 25,
                          fontWeight: FontWeight.bold,
                          color: Color(0xFF2E7D32),
                        ),
                      ),
                    ],
                  ),
                ),

                Expanded(
                  child: ListView.builder(
                    itemCount: orderHistory.length,
                    itemBuilder: (context, index) {
                      final order =
                          orderHistory[index];

                      DateTime date =
                          order["date"] as DateTime;

                      double amount =
                          order["total"] as double;

                      return Card(
                        margin: const EdgeInsets.symmetric(
                          horizontal: 15,
                          vertical: 6,
                        ),
                        child: ListTile(
                          leading: const CircleAvatar(
                            backgroundColor:
                                Color(0xFFE8F5E9),
                            child: Icon(
                              Icons.receipt,
                              color: Color(0xFF2E7D32),
                            ),
                          ),

                          title: Text(
                            "Order ${index + 1}",
                            style: const TextStyle(
                              fontWeight: FontWeight.bold,
                            ),
                          ),

                          subtitle: Text(
                            "${date.day}/${date.month}/${date.year}",
                          ),

                          trailing: Text(
                            "₹${amount.toStringAsFixed(2)}",
                            style: const TextStyle(
                              fontWeight: FontWeight.bold,
                              color: Color(0xFF2E7D32),
                            ),
                          ),
                        ),
                      );
                    },
                  ),
                ),
              ],
            ),
    );
  }
}

// ======================================================
// PROFILE PAGE
// ======================================================

class ProfilePage extends StatelessWidget {
  final String email;

  const ProfilePage({
    super.key,
    required this.email,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("My Profile"),
      ),

      body: Padding(
        padding: const EdgeInsets.all(25),
        child: Column(
          children: [
            const CircleAvatar(
              radius: 55,
              backgroundColor: Color(0xFFE8F5E9),
              child: Icon(
                Icons.person,
                size: 65,
                color: Color(0xFF2E7D32),
              ),
            ),

            const SizedBox(height: 20),

            const Text(
              "Student",
              style: TextStyle(
                fontSize: 25,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 8),

            Text(
              email,
              style: const TextStyle(
                fontSize: 16,
                color: Colors.grey,
              ),
            ),

            const SizedBox(height: 40),

            SizedBox(
              width: double.infinity,
              height: 50,
              child: ElevatedButton.icon(
                onPressed: () {
                  Navigator.pushAndRemoveUntil(
                    context,
                    MaterialPageRoute(
                      builder: (context) =>
                          const LogoutPage(),
                    ),
                    (route) => false,
                  );
                },
                icon: const Icon(Icons.logout),
                label: const Text("LOGOUT"),
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.red,
                  foregroundColor: Colors.white,
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

// ======================================================
// LOGOUT PAGE
// ======================================================

class LogoutPage extends StatelessWidget {
  const LogoutPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: const Color(0xFFE8F5E9),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(30),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Icon(
                Icons.logout,
                size: 90,
                color: Color(0xFF2E7D32),
              ),

              const SizedBox(height: 20),

              const Text(
                "You have been logged out",
                textAlign: TextAlign.center,
                style: TextStyle(
                  fontSize: 25,
                  fontWeight: FontWeight.bold,
                  color: Color(0xFF2E7D32),
                ),
              ),

              const SizedBox(height: 30),

              SizedBox(
                width: double.infinity,
                height: 50,
                child: ElevatedButton(
                  onPressed: () {
                    Navigator.pushReplacement(
                      context,
                      MaterialPageRoute(
                        builder: (context) =>
                            const LoginPage(),
                      ),
                    );
                  },
                  style: ElevatedButton.styleFrom(
                    backgroundColor:
                        const Color(0xFF2E7D32),
                    foregroundColor: Colors.white,
                  ),
                  child: const Text(
                    "LOGIN AGAIN",
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
