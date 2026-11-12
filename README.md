import 'package:flutter/material.dart';

void main() {
  runApp(const CarePointApp());
}

const navy = Color(0xFF123B67);
const blue = Color(0xFF2374C6);
const paleBlue = Color(0xFFEAF3FC);
const canvas = Color(0xFFF5F8FC);
const teal = Color(0xFF20A89A);

class CarePointApp extends StatelessWidget {
  const CarePointApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'CarePoint HMS',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        useMaterial3: true,
        scaffoldBackgroundColor: canvas,
        colorScheme: ColorScheme.fromSeed(seedColor: blue),
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.white,
          foregroundColor: navy,
          elevation: 0,
        ),
      ),
      home: const LoginScreen(),
    );
  }
}

// ================= HOSPITAL DATABASE =================

class HospitalData {
  static final List<Map<String, String>> patients = [
    {
      'name': 'Ali Hassan',
      'id': 'PT-1001',
      'age': '28',
      'phone': '03001112233',
      'status': 'Active'
    },
    {
      'name': 'Fatima Noor',
      'id': 'PT-1002',
      'age': '35',
      'phone': '03014445566',
      'status': 'Active'
    },
  ];

  static final List<Map<String, String>> doctors = [
    {
      'name': 'Dr. Ayesha Khan',
      'specialty': 'Cardiology',
      'phone': '03001234567',
      'status': 'Available'
    },
    {
      'name': 'Dr. Ahmed Raza',
      'specialty': 'Neurology',
      'phone': '03012223344',
      'status': 'Available'
    },
  ];

  static final List<Map<String, String>> appointments = [
    {
      'patient': 'Ali Hassan',
      'doctor': 'Dr. Ayesha Khan',
      'date': '12 Nov 2026',
      'time': '10:00 AM',
      'status': 'Confirmed'
    },
  ];

  static final List<Map<String, String>> doctorFees = [
    {
      'doctor': 'Dr. Ayesha Khan',
      'appointment': 'Ali Hassan',
      'fee': '2500',
      'paid': 'Paid',
      'date': '12 Nov 2026'
    },
    {
      'doctor': 'Dr. Ahmed Raza',
      'appointment': 'Fatima Noor',
      'fee': '3000',
      'paid': 'Pending',
      'date': '12 Nov 2026'
    },
  ];

  static final List<Map<String, String>> medicines = [
    {
      'name': 'Paracetamol',
      'stock': '120',
      'price': '25',
      'category': 'Tablet'
    },
  ];

  static final List<Map<String, String>> bills = [
    {
      'patient': 'Ali Hassan',
      'invoice': 'INV-001',
      'amount': '2500',
      'status': 'Paid'
    },
  ];

  static final List<Map<String, String>> history = [
    {
      'patient': 'Ali Hassan',
      'details': 'Routine Checkup',
      'date': '10 Nov 2026'
    },
  ];
}

// ================= LOGIN SCREEN =================

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final email = TextEditingController(
    text: 'admin@carepoint.com',
  );

  final password = TextEditingController(
    text: '123456',
  );

  String role = 'Admin';
  bool hidePassword = true;

  void login() {
    if (email.text.isEmpty || password.text.length < 6) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Enter valid email and password'),
        ),
      );
      return;
    }

    Navigator.pushReplacement(
      context,
      MaterialPageRoute(
        builder: (_) => DashboardScreen(
          role: role,
          email: email.text,
          name: role == 'Admin'
              ? 'Administrator'
              : role == 'Doctor'
                  ? 'Dr. Ahmed'
                  : 'Patient',
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Center(
          child: SingleChildScrollView(
            padding: const EdgeInsets.all(24),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                const CircleAvatar(
                  radius: 45,
                  backgroundColor: paleBlue,
                  child: Icon(
                    Icons.local_hospital,
                    color: blue,
                    size: 48,
                  ),
                ),

                const SizedBox(height: 25),

                const Text(
                  'Welcome to CarePoint',
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 25,
                    fontWeight: FontWeight.bold,
                    color: navy,
                  ),
                ),

                const SizedBox(height: 8),

                const Text(
                  'Hospital Management System',
                  textAlign: TextAlign.center,
                ),

                const SizedBox(height: 30),

                TextField(
                  controller: email,
                  decoration: const InputDecoration(
                    labelText: 'Email',
                    prefixIcon: Icon(Icons.email),
                    border: OutlineInputBorder(),
                  ),
                ),

                const SizedBox(height: 15),

                TextField(
                  controller: password,
                  obscureText: hidePassword,
                  decoration: InputDecoration(
                    labelText: 'Password',
                    prefixIcon: const Icon(Icons.lock),
                    border: const OutlineInputBorder(),
                    suffixIcon: IconButton(
                      onPressed: () {
                        setState(() {
                          hidePassword = !hidePassword;
                        });
                      },
                      icon: Icon(
                        hidePassword
                            ? Icons.visibility
                            : Icons.visibility_off,
                      ),
                    ),
                  ),
                ),

                const SizedBox(height: 15),

                DropdownButtonFormField<String>(
                  value: role,
                  decoration: const InputDecoration(
                    labelText: 'Login as',
                    border: OutlineInputBorder(),
                  ),
                  items: const [
                    'Admin',
                    'Doctor',
                    'Patient',
                  ].map((r) {
                    return DropdownMenuItem(
                      value: r,
                      child: Text(r),
                    );
                  }).toList(),
                  onChanged: (v) {
                    setState(() {
                      role = v!;
                    });
                  },
                ),

                const SizedBox(height: 25),

                SizedBox(
                  height: 52,
                  child: FilledButton(
                    onPressed: login,
                    child: const Text('LOGIN'),
                  ),
                ),

                TextButton(
                  onPressed: () {
                    Navigator.push(
                      context,
                      MaterialPageRoute(
                        builder: (_) => const SignupScreen(),
                      ),
                    );
                  },
                  child: const Text('Create New Account'),
                ),

                const Text(
                  'Demo: admin@carepoint.com\nPassword: 123456',
                  textAlign: TextAlign.center,
                  style: TextStyle(color: Colors.blueGrey),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

// ================= SIGNUP SCREEN =================

class SignupScreen extends StatefulWidget {
  const SignupScreen({super.key});

  @override
  State<SignupScreen> createState() => _SignupScreenState();
}

class _SignupScreenState extends State<SignupScreen> {
  final name = TextEditingController();
  final email = TextEditingController();
  final password = TextEditingController();

  String role = 'Patient';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Create Account'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: ListView(
          children: [
            const Text(
              'Register New Account',
              style: TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
                color: navy,
              ),
            ),

            const SizedBox(height: 25),

            TextField(
              controller: name,
              decoration: const InputDecoration(
                labelText: 'Full Name',
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: email,
              decoration: const InputDecoration(
                labelText: 'Email',
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: password,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: 'Password',
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 15),

            DropdownButtonFormField<String>(
              value: role,
              decoration: const InputDecoration(
                labelText: 'Account Type',
                border: OutlineInputBorder(),
              ),
              items: const [
                'Patient',
                'Doctor',
              ].map((r) {
                return DropdownMenuItem(
                  value: r,
                  child: Text(r),
                );
              }).toList(),
              onChanged: (v) {
                setState(() {
                  role = v!;
                });
              },
            ),

            const SizedBox(height: 25),

            FilledButton(
              onPressed: () {
                if (name.text.isEmpty ||
                    !email.text.contains('@') ||
                    password.text.length < 6) {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(
                      content: Text('Please enter valid details'),
                    ),
                  );
                  return;
                }

                Navigator.pushAndRemoveUntil(
                  context,
                  MaterialPageRoute(
                    builder: (_) => DashboardScreen(
                      role: role,
                      email: email.text,
                      name: name.text,
                    ),
                  ),
                  (route) => false,
                );
              },
              child: const Text('SIGN UP'),
            ),
          ],
        ),
      ),
    );
  }
}
