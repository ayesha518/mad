import React, { useState } from "react";
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  StyleSheet,
  Switch,
  FlatList,
  ScrollView,
} from "react-native";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";

// Login Screen
const LoginScreen = ({ navigation }) => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = () => {
    console.log("Logged in with:", email);
    navigation.navigate("DashboardScreen");
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login</Text>
      <TextInput
        style={styles.input}
        placeholder="Email"
        placeholderTextColor="#999"
        onChangeText={(text) => setEmail(text)}
        value={email}
        keyboardType="email-address"
      />
      <TextInput
        style={styles.input}
        placeholder="Password"
        placeholderTextColor="#999"
        onChangeText={(text) => setPassword(text)}
        value={password}
        secureTextEntry={true}
      />
      <TouchableOpacity style={styles.button} onPress={handleLogin}>
        <Text style={styles.buttonText}>LOGIN</Text>
      </TouchableOpacity>
      <Text
        style={styles.linkText}
        onPress={() => navigation.navigate("SignUpScreen")}
      >
        Don't have an account? Sign Up
      </Text>
    </View>
  );
};

// Sign-Up Screen
const SignUpScreen = ({ navigation }) => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [confirmPassword, setConfirmPassword] = useState("");

  const handleSignUp = () => {
    console.log("Signed up with:", email);
    navigation.navigate("LoginScreen");
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Sign Up</Text>
      <TextInput
        style={styles.input}
        placeholder="Email"
        placeholderTextColor="#999"
        onChangeText={(text) => setEmail(text)}
        value={email}
        keyboardType="email-address"
      />
      <TextInput
        style={styles.input}
        placeholder="Password"
        placeholderTextColor="#999"
        onChangeText={(text) => setPassword(text)}
        value={password}
        secureTextEntry={true}
      />
      <TextInput
        style={styles.input}
        placeholder="Confirm Password"
        placeholderTextColor="#999"
        onChangeText={(text) => setConfirmPassword(text)}
        value={confirmPassword}
        secureTextEntry={true}
      />
      <TouchableOpacity style={styles.button} onPress={handleSignUp}>
        <Text style={styles.buttonText}>SIGN UP</Text>
      </TouchableOpacity>
      <Text
        style={styles.linkText}
        onPress={() => navigation.navigate("LoginScreen")}
      >
        Already have an account? Login
      </Text>
    </View>
  );
};

// Dashboard Screen
const DashboardScreen = ({ navigation }) => {
  const [isLightMode, setIsLightMode] = useState(false);

  const toggleLightMode = () => setIsLightMode((prev) => !prev);

  return (
    <View
      style={[
        styles.container,
        isLightMode ? styles.lightBackground : styles.darkBackground,
      ]}
    >
      <Text style={styles.title}>Local Service Finder</Text>
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate("ServiceSearchScreen")}
      >
        <Text style={styles.buttonText}>Search Service</Text>
      </TouchableOpacity>
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate("ServiceListScreen")}
      >
        <Text style={styles.buttonText}>View Services</Text>
      </TouchableOpacity>
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate("ChatScreen")}
      >
        <Text style={styles.buttonText}>Chat with Providers</Text>
      </TouchableOpacity>
      <Switch value={isLightMode} onValueChange={toggleLightMode} />
      <Text style={styles.lightText}>Light Mode</Text>
      <TouchableOpacity
        style={styles.logoutButton}
        onPress={() => navigation.navigate("LoginScreen")}
      >
        <Text style={styles.buttonText}>LOGOUT</Text>
      </TouchableOpacity>
    </View>
  );
};

// Service Search Screen
const ServiceSearchScreen = () => {
  const [query, setQuery] = useState("");
  const [filteredServices, setFilteredServices] = useState([]);

  const services = [
    { id: "1", name: "Plumbing" },
    { id: "2", name: "Electrician" },
    { id: "3", name: "Cleaning" },
    { id: "4", name: "Carpentry" },
    { id: "5", name: "AC Repair" },
  ];

  const handleSearch = () => {
    const results = services.filter((service) =>
      service.name.toLowerCase().includes(query.toLowerCase())
    );
    setFilteredServices(results);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Search for Services</Text>
      <TextInput
        style={styles.input}
        placeholder="Enter service name"
        placeholderTextColor="#999"
        onChangeText={(text) => setQuery(text)}
        value={query}
      />
      <TouchableOpacity style={styles.button} onPress={handleSearch}>
        <Text style={styles.buttonText}>SEARCH</Text>
      </TouchableOpacity>

      {filteredServices.length > 0 ? (
        <FlatList
          data={filteredServices}
          keyExtractor={(item) => item.id}
          renderItem={({ item }) => (
            <View style={styles.serviceItem}>
              <Text style={styles.serviceText}>{item.name}</Text>
            </View>
          )}
        />
      ) : (
        <Text>No services found.</Text>
      )}
    </View>
  );
};

// Service List Screen
const ServiceListScreen = () => {
  const services = [
    { id: "1", name: "Plumbing" },
    { id: "2", name: "Electrician" },
    { id: "3", name: "Cleaning" },
    { id: "4", name: "Carpentry" },
    { id: "5", name: "AC Repair" },
  ];

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Available Services</Text>
      <FlatList
        data={services}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.serviceItem}>
            <Text style={styles.serviceText}>{item.name}</Text>
          </View>
        )}
      />
    </View>
  );
};

// Chat Screen
const ChatScreen = () => {
  const [message, setMessage] = useState("");
  const [messages, setMessages] = useState([
    { id: "1", text: "Hello, how can I help you?" },
    { id: "2", text: "I am looking for plumbing services." },
  ]);

  const handleSendMessage = () => {
    if (message) {
      setMessages((prevMessages) => [
        ...prevMessages,
        { id: (prevMessages.length + 1).toString(), text: message },
      ]);
      setMessage(""); // Clear input field after sending message
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Chat with Providers</Text>
      <ScrollView style={styles.chatContainer}>
        {messages.map((msg) => (
          <View key={msg.id} style={styles.message}>
            <Text>{msg.text}</Text>
          </View>
        ))}
      </ScrollView>
      <TextInput
        style={styles.input}
        placeholder="Type your message"
        onChangeText={(text) => setMessage(text)}
        value={message}
      />
      <TouchableOpacity style={styles.button} onPress={handleSendMessage}>
        <Text style={styles.buttonText}>SEND</Text>
      </TouchableOpacity>
    </View>
  );
};

// Navigation Setup
const Stack = createStackNavigator();
const Tab = createBottomTabNavigator();

const DashboardTabs = () => {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Search" component={ServiceSearchScreen} />
      <Tab.Screen name="Services" component={ServiceListScreen} />
      <Tab.Screen name="Chat" component={ChatScreen} />
    </Tab.Navigator>
  );
};

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="LoginScreen">
        <Stack.Screen name="LoginScreen" component={LoginScreen} />
        <Stack.Screen name="SignUpScreen" component={SignUpScreen} />
        <Stack.Screen name="DashboardScreen" component={DashboardTabs} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};

export default App;

// Styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    padding: 20,
  },
  title: {
    fontSize: 28,
    fontWeight: "bold",
    marginBottom: 20,
    color: "#000",
  },
  input: {
    width: "90%",
    height: 50,
    borderColor: "#ccc",
    borderWidth: 1,
    borderRadius: 5,
    paddingHorizontal: 10,
    marginBottom: 15,
    color: "#000",
  },
  button: {
    width: "90%",
    backgroundColor: "#007bff",
    padding: 15,
    borderRadius: 5,
    alignItems: "center",
    marginBottom: 15,
  },
  buttonText: {
    color: "#fff",
    fontSize: 16,
    fontWeight: "bold",
  },
  linkText: {
    fontSize: 14,
    color: "#007bff",
  },
  lightBackground: {
    backgroundColor: "#fff",
  },
  darkBackground: {
    backgroundColor: "#000",
  },
  lightText: {
    marginTop: 10,
    color: "#000",
  },
  logoutButton: {
    width: "90%",
    backgroundColor: "#dc3545",
    padding: 15,
    borderRadius: 5,
    alignItems: "center",
    marginTop: 30,
  },
  serviceItem: {
    backgroundColor: "#f8f8f8",
    padding: 15,
    borderRadius: 5,
    marginBottom: 10,
    width: "100%",
  },
  serviceText: {
    fontSize: 18,
    color: "#333",
  },
  chatContainer: {
    width: "100%",
    height: 250,
    marginBottom: 10,
    paddingVertical: 10,
  },
  message: {
    backgroundColor: "#f1f1f1",
    padding: 10,
    marginBottom: 5,
    borderRadius: 5,
    width: "90%",
  },
});
