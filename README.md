# Pengembangan-Aplikasi-Mobil-Berbasis-Android


import React, { useState } from 'react';
import {
  SafeAreaView,
  View,
  Text,
  TextInput,
  TouchableOpacity,
  StyleSheet,
  Alert,
  KeyboardAvoidingView,
  Platform,
  ScrollView,
} from 'react-native';

export default function App() {
  const [page, setPage] = useState('login'); // login, register, dashboard
  const [nama, setNama] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [confirmPassword, setConfirmPassword] = useState('');

  // Simulasi user
  const [registeredUser, setRegisteredUser] = useState(null);

  // Login Handler
  const handleLogin = () => {
    if (!email || !password) {
      Alert.alert('Peringatan', 'Email dan password harus diisi');
      return;
    }

    if (
      registeredUser &&
      email === registeredUser.email &&
      password === registeredUser.password
    ) {
      Alert.alert('Login Berhasil', `Selamat datang, ${registeredUser.nama}`);
      setPage('dashboard');
    } else {
      Alert.alert('Gagal', 'Email atau password salah');
    }
  };

  // Register Handler
  const handleRegister = () => {
    if (!nama || !email || !password || !confirmPassword) {
      Alert.alert('Peringatan', 'Semua kolom wajib diisi!');
      return;
    }
    if (password !== confirmPassword) {
      Alert.alert('Gagal', 'Password tidak sama');
      return;
    }

    setRegisteredUser({ nama, email, password });
    Alert.alert('Berhasil', 'Registrasi berhasil, silakan login');
    setPage('login');
    setNama('');
    setEmail('');
    setPassword('');
    setConfirmPassword('');
  };

  // Dashboard Handler
  const handleLogout = () => {
    setPage('login');
    setEmail('');
    setPassword('');
  };

  return (
    <SafeAreaView style={styles.safe}>
      <KeyboardAvoidingView
        behavior={Platform.OS === 'ios' ? 'padding' : undefined}
        style={styles.container}
      >
        <ScrollView contentContainerStyle={styles.inner}>
          {page === 'login' && (
            <>
              <Text style={styles.title}>Login</Text>
              <TextInput
                style={styles.input}
                placeholder="Email"
                value={email}
                onChangeText={setEmail}
                keyboardType="email-address"
                autoCapitalize="none"
              />
              <TextInput
                style={styles.input}
                placeholder="Password"
                value={password}
                onChangeText={setPassword}
                secureTextEntry
              />
              <TouchableOpacity style={styles.button} onPress={handleLogin}>
                <Text style={styles.buttonText}>Login</Text>
              </TouchableOpacity>
              <TouchableOpacity onPress={() => setPage('register')}>
                <Text style={styles.toggleText}>Belum punya akun? Daftar</Text>
              </TouchableOpacity>
            </>
          )}

          {page === 'register' && (
            <>
              <Text style={styles.title}>Register</Text>
              <TextInput
                style={styles.input}
                placeholder="Nama Lengkap"
                value={nama}
                onChangeText={setNama}
              />
              <TextInput
                style={styles.input}
                placeholder="Email"
                value={email}
                onChangeText={setEmail}
                keyboardType="email-address"
                autoCapitalize="none"
              />
              <TextInput
                style={styles.input}
                placeholder="Password"
                value={password}
                onChangeText={setPassword}
                secureTextEntry
              />
              <TextInput
                style={styles.input}
                placeholder="Konfirmasi Password"
                value={confirmPassword}
                onChangeText={setConfirmPassword}
                secureTextEntry
              />
              <TouchableOpacity style={styles.button} onPress={handleRegister}>
                <Text style={styles.buttonText}>Daftar</Text>
              </TouchableOpacity>
              <TouchableOpacity onPress={() => setPage('login')}>
                <Text style={styles.toggleText}>Sudah punya akun? Login</Text>
              </TouchableOpacity>
            </>
          )}

          {page === 'dashboard' && (
            <>
              <Text style={styles.title}>Dashboard</Text>
              <Text style={styles.welcome}>Halo, {registeredUser?.nama}</Text>
              <TouchableOpacity style={[styles.button, { backgroundColor: 'red' }]} onPress={handleLogout}>
                <Text style={styles.buttonText}>Logout</Text>
              </TouchableOpacity>
            </>
          )}
        </ScrollView>
      </KeyboardAvoidingView>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  safe: {
    flex: 1,
    backgroundColor: '#fff',
  },
  container: {
    flex: 1,
    paddingHorizontal: 20,
  },
  inner: {
    justifyContent: 'center',
    paddingVertical: 60,
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    alignSelf: 'center',
    marginBottom: 30,
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    marginBottom: 30,
    color: '#333',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    paddingHorizontal: 14,
    paddingVertical: 10,
    borderRadius: 8,
    marginBottom: 15,
  },
  button: {
    backgroundColor: '#007AFF',
    padding: 14,
    borderRadius: 8,
    alignItems: 'center',
    marginBottom: 15,
  },
  buttonText: {
    color: '#fff',
    fontWeight: '600',
    fontSize: 16,
  },
  toggleText: {
    color: '#007AFF',
    textAlign: 'center',
  },
});
