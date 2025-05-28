# Renton Technical College CSI-248

<div align="center">
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 5: Building a Device Info App with Expo APIs</h3>
</div>

## Overview

In this guided activity, you'll build a Device Info App that monitors battery status and network connectivity using Expo's device APIs. The app will feature:
- Tab-based navigation
- Real-time battery monitoring
- Network status tracking

## Resources:
https://docs.expo.dev/versions/latest/sdk/battery/
https://docs.expo.dev/versions/latest/sdk/network/


## Getting Started with GitHub

1. Clone this repository to your machine using Github Desktop:
   - Click the green "Code" button on the repository page
   - Select "Open with GitHub Desktop"
   - Choose your local path
   - Click "Clone"

2. In GitHub Desktop:
   - Click "For my own purposes" when prompted
   - Note the local path where you cloned the repository

3. Open Visual Studio Code:
   - In GitHub Desktop, click "Open in Visual Studio Code"

## Step 1: Setting Up the Project

1. Open your terminal in VS Code (Terminal > New Terminal) and create a new Expo project:

```bash
npx create-expo-app DeviceInfoApp --template blank
cd DeviceInfoApp
```

2. Install the required dependencies:

```bash
# Install navigation dependencies
npx expo install @react-navigation/native @react-navigation/bottom-tabs

# Install device API packages
npx expo install expo-battery expo-network

# Install required navigation utilities
npx expo install react-native-screens react-native-safe-area-context
```

3. Create a screens folder inside of your project.


## Step 2: Basic Screen Setup

First, let's create basic screen components to verify our navigation works. We'll add the complex functionality later.

1. Create `screens/HomeScreen.js`:
```javascript
// Import required components from React Native
import { View, Text, StyleSheet } from 'react-native';
import React from 'react';

// Create the HomeScreen component
export default function HomeScreen() {
  return (
    <View style={styles.container}>
      <Text>Home Screen</Text>
    </View>
  );
}

// Define styles for the component
const styles = StyleSheet.create({
  container: {
    flex: 1,                    // Take up all available space
    justifyContent: 'center',   // Center content vertically
    alignItems: 'center',       // Center content horizontally
  },
});
```

2. Create `screens/BatteryScreen.js`:
```javascript
// Import required components from React Native
import { View, Text, StyleSheet } from 'react-native';
import React from 'react';

// Create the BatteryScreen component
export default function BatteryScreen() {
  return (
    <View style={styles.container}>
      <Text>Battery Screen</Text>
    </View>
  );
}

// Define styles for the component
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

3. Create `screens/NetworkScreen.js`:
```javascript
// Import required components from React Native
import { View, Text, StyleSheet } from 'react-native';
import React from 'react';

// Create the NetworkScreen component
export default function NetworkScreen() {
  return (
    <View style={styles.container}>
      <Text>Network Screen</Text>
    </View>
  );
}

// Define styles for the component
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

## Step 3: Setting Up Navigation

Update your `App.js`:

```javascript
// Import necessary components and libraries
import { StyleSheet } from 'react-native';
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Ionicons } from '@expo/vector-icons';

// Import our screen components
import HomeScreen from './screens/HomeScreen';
import BatteryScreen from './screens/BatteryScreen';
import NetworkScreen from './screens/NetworkScreen';

// Create a Tab Navigator instance
const Tab = createBottomTabNavigator();

export default function App() {
  return (
    // NavigationContainer is required at the root of navigation tree
    <NavigationContainer>
      {/* Tab.Navigator configures the tab bar */}
      <Tab.Navigator
        screenOptions={({ route }) => ({
          // Configure the tab bar icons
          tabBarIcon: ({ focused, color, size }) => {
            let iconName;

            // Choose the icon based on the route name and whether it's focused
            if (route.name === 'Home') {
              iconName = focused ? 'home' : 'home-outline';
            } else if (route.name === 'Battery') {
              iconName = focused ? 'battery-full' : 'battery-full-outline';
            } else if (route.name === 'Network') {
              iconName = focused ? 'wifi' : 'wifi-outline';
            }

            // Return the Ionicons component with the selected icon
            return <Ionicons name={iconName} size={size} color={color} />;
          },
        })}
      >
        {/* Define our screens */}
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Battery" component={BatteryScreen} />
        <Tab.Screen name="Network" component={NetworkScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}
```

## Step 4: Testing the Navigation

1. Start your Expo development server:

```bash
npx expo start
```

2. Testing Checklist:
   - Open the app on your device or simulator using the Expo Go app
   - Verify all three tabs are visible at the bottom
   - Check that each tab has the correct icon
   - Test switching between screens by tapping the tabs
   - Confirm each screen shows its basic text content

3. Create a commit for your working navigation:
   - Open GitHub Desktop
   - Review your changes
   - Enter a commit message: "Initial navigation setup with basic screens"
   - Click "Commit to main"
   - Click "Push origin" to upload your changes

## Step 5: Implementing the Home Screen

Now let's enhance the Home screen with a proper welcome interface. Update `HomeScreen.js`:

```javascript
// Import necessary components from React Native
import { StyleSheet, Text, View } from 'react-native';
import React from 'react';

export default function HomeScreen() {
  return (
    <View style={styles.container}>
      {/* Main title of the app */}
      <Text style={styles.title}>Welcome to Device Info App</Text>
      
      {/* Descriptive subtitle */}
      <Text style={styles.subtitle}>
        Check your battery and network status
      </Text>
    </View>
  );
}

// Define styles for the component
const styles = StyleSheet.create({
  container: {
    flex: 1,                    // Take up all available space
    backgroundColor: '#fff',    // White background
    alignItems: 'center',       // Center items horizontally
    justifyContent: 'center',   // Center items vertically
  },
  title: {
    fontSize: 24,               // Large text size for title
    fontWeight: 'bold',         // Bold text
    marginBottom: 20,           // Space below the title
  },
  subtitle: {
    fontSize: 16,               // Medium text size for subtitle
    color: '#666',             // Gray color for less emphasis
  },
});
```

## Step 6: Commit Updated Home Screen

1. Test the updated Home Screen:
   - Verify the welcome message appears
   - Check that the styling is correct
   - Ensure navigation still works properly

2. Create a commit for your Home Screen updates:
   - Review your changes
   - Enter a commit message: "Implemented styled Home Screen"
   - Click "Commit to main"
   - Click "Push origin" to upload your changes

# Building a Device Info App with Expo APIs - Part 2

## Step 7: Implementing the Battery Monitor

First, let's understand the Battery API from Expo. The [expo-battery](https://docs.expo.dev/versions/latest/sdk/battery/) module provides information about the device's battery state and level.

Update `screens/BatteryScreen.js`:

```javascript
// Import necessary components and hooks from React and React Native
import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet, Platform } from 'react-native';
// Import the Battery module from expo-battery
import * as Battery from 'expo-battery';

export default function BatteryScreen() {
  // State variables to store battery information
  const [batteryLevel, setBatteryLevel] = useState(null);
  const [isCharging, setIsCharging] = useState(false);
  const [batteryState, setBatteryState] = useState('');

  // useEffect hook to handle battery monitoring
  useEffect(() => {
    // Subscribe to battery updates when component mounts
    subscribe();
    // Cleanup function to unsubscribe when component unmounts
    return () => {
      unsubscribe();
    };
  }, []);

  // Function to set up battery monitoring subscriptions
  const subscribe = async () => {
    try {
      // Get initial battery level
      const level = await Battery.getBatteryLevelAsync();
      setBatteryLevel(level);

      // Get initial charging status
      const status = await Battery.getBatteryStateAsync();
      setIsCharging(status === Battery.BatteryState.CHARGING);
      setBatteryState(getBatteryStateLabel(status));

      // Subscribe to battery level changes
      Battery.addBatteryLevelListener(({ batteryLevel }) => {
        setBatteryLevel(batteryLevel);
      });

      // Subscribe to charging state changes
      Battery.addBatteryStateListener(({ batteryState }) => {
        setIsCharging(batteryState === Battery.BatteryState.CHARGING);
        setBatteryState(getBatteryStateLabel(batteryState));
      });
    } catch (error) {
      console.error('Error setting up battery monitoring:', error);
    }
  };

  // Function to cleanup battery monitoring subscriptions
  const unsubscribe = () => {
    Battery.removeBatteryLevelListener();
    Battery.removeBatteryStateListener();
  };

  // Helper function to convert battery state to readable label
  const getBatteryStateLabel = (state) => {
    switch (state) {
      case Battery.BatteryState.UNKNOWN:
        return 'Unknown';
      case Battery.BatteryState.UNPLUGGED:
        return 'Not Charging';
      case Battery.BatteryState.CHARGING:
        return 'Charging';
      case Battery.BatteryState.FULL:
        return 'Full';
      default:
        return 'Unknown';
    }
  };

  // Helper function to determine battery indicator color
  const getBatteryColor = (level) => {
    if (level > 0.5) return '#4CAF50';  // Green for high battery
    if (level > 0.2) return '#FFC107';  // Yellow for medium battery
    return '#F44336';                   // Red for low battery
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Battery Monitor</Text>

      {/* Battery Level Indicator */}
      <View style={styles.batteryContainer}>
        <View style={styles.batteryOuter}>
          <View
            style={[
              styles.batteryInner,
              {
                width: `${(batteryLevel || 0) * 100}%`,
                backgroundColor: getBatteryColor(batteryLevel),
              },
            ]}
          />
        </View>
        <View style={styles.batteryTip} />
      </View>

      {/* Battery Stats */}
      <View style={styles.statsContainer}>
        <Text style={styles.percentage}>
          {batteryLevel ? `${Math.round(batteryLevel * 100)}%` : 'Loading...'}
        </Text>
        <Text style={styles.status}>Status: {batteryState}</Text>
        {isCharging && <Text style={styles.charging}>⚡ Charging</Text>}
      </View>

      <Text style={styles.note}>
        This app demonstrates the use of {Platform.OS === 'ios' ? 'iOS' : 'Android'} battery API
      </Text>
    </View>
  );
}

// Styles for the Battery Screen
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 40,
  },
  batteryContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 30,
  },
  batteryOuter: {
    width: 200,
    height: 80,
    borderWidth: 3,
    borderColor: '#333',
    borderRadius: 10,
    overflow: 'hidden',
    backgroundColor: '#fff',
  },
  batteryInner: {
    height: '100%',
    backgroundColor: '#4CAF50',
  },
  batteryTip: {
    width: 10,
    height: 30,
    backgroundColor: '#333',
    borderTopRightRadius: 5,
    borderBottomRightRadius: 5,
  },
  statsContainer: {
    alignItems: 'center',
  },
  percentage: {
    fontSize: 48,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  status: {
    fontSize: 18,
    color: '#666',
    marginBottom: 5,
  },
  charging: {
    fontSize: 18,
    color: '#4CAF50',
    fontWeight: 'bold',
  },
  note: {
    position: 'absolute',
    bottom: 40,
    color: '#666',
    fontSize: 14,
  },
});
```

## Step 8: Testing the Battery Screen

1. Test the Battery Screen implementation:
   - Run the app and navigate to the Battery tab
   - Verify the battery level indicator appears
   - Check that the percentage is displayed correctly
   - Plug in your device to test charging status changes
   - Unplug to verify the status updates

2. Create a commit for your Battery Screen:   

   - Review your changes
   - Enter commit message: "Implemented Battery monitoring screen"
   - Click "Commit to main"
   - Click "Push origin"

## Step 9: Implementing the Network Monitor

The [expo-network](https://docs.expo.dev/versions/latest/sdk/network/) module provides information about the device's network state and connection type.

Update `screens/NetworkScreen.js`:

```javascript
// Import necessary React and React Native components
import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet, ScrollView } from 'react-native';
// Import the Network module from Expo
import * as Network from 'expo-network';

export default function NetworkScreen() {
  // Define state to store network information
  const [networkState, setNetworkState] = useState({
    isConnected: false,        // Is the device connected to a network?
    type: null,                // Type of network connection (WIFI, CELLULAR, etc.)
    isInternetReachable: false, // Is internet actually accessible?
    ip: null,                  // Device's IP address
  });

  // Set up network monitoring when component mounts
  useEffect(() => {
    // Check network status immediately
    checkNetwork();

    // Set up interval to check network status every 5 seconds
    const interval = setInterval(checkNetwork, 5000);

    // Cleanup: clear interval when component unmounts
    return () => clearInterval(interval);
  }, []); // Empty dependency array means this runs once on mount

  // Function to check and update network status
  const checkNetwork = async () => {
    try {
      // Get current network state using Expo's Network API
      const networkState = await Network.getNetworkStateAsync();
      // Get device's IP address
      const ip = await Network.getIpAddressAsync();

      // Update state with new network information
      setNetworkState({
        isConnected: networkState.isConnected,
        type: networkState.type,
        isInternetReachable: networkState.isInternetReachable,
        ip,
      });
    } catch (error) {
      console.error('Error checking network:', error);
    }
  };

  // Function to get appropriate icon for network type
  const getNetworkTypeIcon = (type) => {
    // Map network types to emoji icons
    const icons = {
      WIFI: '📶',
      CELLULAR: '📱',
      BLUETOOTH: '🦷',
      ETHERNET: '🔌',
      VPN: '🔒',
      UNKNOWN: '❓',
    };
    return icons[type] || '❓';
  };

  return (
    <ScrollView style={styles.container}>
      {/* Network Status Card */}
      <View style={styles.card}>
        <Text style={styles.title}>Network Status</Text>

        {/* Connection Status Indicator */}
        <View style={styles.statusContainer}>
          <Text
            style={[
              styles.connectionDot,
              { backgroundColor: networkState.isConnected ? 'green' : 'red' },
            ]}
          >
            ⬤
          </Text>
          <Text style={styles.statusText}>
            {networkState.isConnected ? 'Connected' : 'Disconnected'}
          </Text>
        </View>

        {/* Network Information Rows */}
        <View style={styles.infoRow}>
          <Text style={styles.label}>Network Type:</Text>
          <Text style={styles.value}>
            {getNetworkTypeIcon(networkState.type)} {networkState.type || 'Unknown'}
          </Text>
        </View>

        <View style={styles.infoRow}>
          <Text style={styles.label}>Internet Access:</Text>
          <Text
            style={[
              styles.value,
              { color: networkState.isInternetReachable ? 'green' : 'red' },
            ]}
          >
            {networkState.isInternetReachable ? 'Available' : 'Not Available'}
          </Text>
        </View>

        <View style={styles.infoRow}>
          <Text style={styles.label}>IP Address:</Text>
          <Text style={styles.value}>{networkState.ip || 'Unknown'}</Text>
        </View>
      </View>

      {/* Tips Card */}
      <View style={[styles.card, styles.tipsCard]}>
        <Text style={styles.tipsTitle}>Network Tips</Text>
        <Text style={styles.tipText}>• Turn off WiFi to test cellular connection</Text>
        <Text style={styles.tipText}>• Enable Airplane mode to test offline state</Text>
        <Text style={styles.tipText}>• Connect to VPN to see network type change</Text>
      </View>
    </ScrollView>
  );
}

// Define styles for the component
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: 'whitesmoke',
    padding: 20,
  },
  card: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
    marginBottom: 20,
    // Add shadow for iOS
    shadowColor: 'black',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    // Add elevation for Android
    elevation: 3,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
    textAlign: 'center',
  },
  statusContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'center',
    marginBottom: 30,
  },
  connectionDot: {
    fontSize: 24,
    marginRight: 10,
  },
  statusText: {
    fontSize: 20,
    fontWeight: 'bold',
  },
  infoRow: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    paddingVertical: 10,
    borderBottomWidth: 1,
    borderBottomColor: 'lightgray',
  },
  label: {
    fontSize: 16,
    color: 'dimgray',
  },
  value: {
    fontSize: 16,
    fontWeight: '500',
  },
  tipsCard: {
    backgroundColor: 'aliceblue',
  },
  tipsTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 15,
    color: 'navy',
  },
  tipText: {
    fontSize: 14,
    color: 'darkslategray',
    marginBottom: 8,
  },
});
```

## Step 10: Testing the Network Screen

1. Test the Network Screen implementation:
   - Run the app and navigate to the Network tab
   - Verify the connection status indicator works
   - Test different network states:
     * Turn WiFi on/off
     * Enable/disable mobile data
     * Enable/disable airplane mode
   - Check that the IP address is displayed correctly
   - Verify the tips card is visible and properly styled

2. Create a commit for your Network Screen implementation:

Or using GitHub Desktop:
- Review your changes
- Enter commit message: "Guided Activity 5 Complete"
- Click "Commit to main"
- Click "Push origin"
