# Renton Technical College CSI-248

<div align="center">
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 6: Building a Material Design App with React Native Paper</h3>
</div>

## Overview

In this guided activity, you'll build a comprehensive Material Design App using React Native Paper, exploring theming capabilities and popular components. The app will feature:
- Tab-based navigation with Material Design styling
- Custom theming with light/dark mode toggle
- Comprehensive showcase of Paper components
- Interactive UI elements and forms

## Resources:
https://callstack.github.io/react-native-paper/
https://callstack.github.io/react-native-paper/docs/guides/theming
https://material.io/design


## Getting Started with GitHub

1. Clone this repository to your machine

2. Open Visual Studio Code:

## Step 1: Setting Up the Project

1. Open your terminal in VS Code (Terminal > New Terminal) and create a new Expo project:

```bash
npx create-expo-app MaterialDesignApp --template blank
cd MaterialDesignApp
```

2. Install the required dependencies:

```bash
# Install React Native Paper - Material Design components library
npm install react-native-paper

# Install navigation dependencies
npx expo install @react-navigation/native @react-navigation/bottom-tabs

# Install vector icons for Material Design icons
npx expo install @expo/vector-icons

# Install required navigation utilities
npx expo install react-native-screens react-native-safe-area-context

# Install async storage for theme persistence
npx expo install @react-native-async-storage/async-storage
```

3. Create a screens folder inside of your project.

## Step 2: Setting Up the Theme Provider

React Native Paper uses a theme system based on Material Design principles. Let's set up our app with a configurable theme.

Update your `App.js`:

```javascript
// Import necessary components and libraries
import React, { useState, useEffect } from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { StatusBar } from 'expo-status-bar';
import AsyncStorage from '@react-native-async-storage/async-storage';

// Import React Native Paper components and theming
import {
  Provider as PaperProvider,
  MD3LightTheme,
  MD3DarkTheme,
  adaptNavigationTheme,
  Text,
} from 'react-native-paper';

// Import our screen components (we'll create these next)
import HomeScreen from './screens/HomeScreen';
import ComponentsScreen from './screens/ComponentsScreen';
import FormsScreen from './screens/FormsScreen';
import ThemeScreen from './screens/ThemeScreen';

// Create navigation themes that match Paper themes
const { LightTheme, DarkTheme } = adaptNavigationTheme({
  reactNavigationLight: { colors: { background: MD3LightTheme.colors.background } },
  reactNavigationDark: { colors: { background: MD3DarkTheme.colors.background } },
});

// Create a Tab Navigator instance
const Tab = createBottomTabNavigator();

export default function App() {
  // State to manage dark mode toggle
  const [isDarkTheme, setIsDarkTheme] = useState(false);

  // Load saved theme preference on app startup
  useEffect(() => {
    loadThemePreference();
  }, []);

  // Function to load theme preference from storage
  const loadThemePreference = async () => {
    try {
      const savedTheme = await AsyncStorage.getItem('isDarkTheme');
      if (savedTheme !== null) {
        setIsDarkTheme(JSON.parse(savedTheme));
      }
    } catch (error) {
      console.error('Error loading theme preference:', error);
    }
  };

  // Function to toggle between light and dark themes
  const toggleTheme = async () => {
    const newTheme = !isDarkTheme;
    setIsDarkTheme(newTheme);
    try {
      await AsyncStorage.setItem('isDarkTheme', JSON.stringify(newTheme));
    } catch (error) {
      console.error('Error saving theme preference:', error);
    }
  };

  // Select the appropriate theme based on current mode
  const paperTheme = isDarkTheme ? MD3DarkTheme : MD3LightTheme;
  const navigationTheme = {
    ...(isDarkTheme ? DarkTheme : LightTheme),
    colors: {
      ...((isDarkTheme ? DarkTheme : LightTheme).colors),
      background: paperTheme.colors.background,
    },
    fonts: paperTheme.fonts, // Ensure fonts are present for navigation
  };

  return (
    // PaperProvider wraps the entire app and provides theming context
    <PaperProvider theme={paperTheme}>
      <NavigationContainer theme={navigationTheme}>
        {/* StatusBar adapts to the current theme */}
        <StatusBar style={isDarkTheme ? 'light' : 'dark'} />
        
        <Tab.Navigator
          screenOptions={({ route }) => ({
            // Configure the tab bar icons using Material Design icons
            tabBarIcon: ({ focused, color, size }) => {
              let iconName;

              // Choose icons based on route name and focus state
              if (route.name === 'Home') {
                iconName = focused ? 'home' : 'home-outline';
              } else if (route.name === 'Components') {
                iconName = focused ? 'view-dashboard' : 'view-dashboard-outline';
              } else if (route.name === 'Forms') {
                iconName = 'form-select';
              } else if (route.name === 'Theme') {
                iconName = focused ? 'palette' : 'palette-outline';
              }

              // Use Expo vector icons (Material Community Icons)
              const { MaterialCommunityIcons } = require('@expo/vector-icons');
              return <MaterialCommunityIcons name={iconName} size={size} color={color} />;
            },
          })}
        >
          {/* Define our screens and pass theme toggle function */}
          <Tab.Screen name="Home" component={HomeScreen} />
          <Tab.Screen name="Components" component={ComponentsScreen} />
          <Tab.Screen 
            name="Forms" 
            children={() => (
              <FormsScreen 
                isDarkTheme={isDarkTheme} 
                toggleTheme={toggleTheme} 
              />
            )}
          />
          <Tab.Screen 
            name="Theme" 
            children={() => (
              <ThemeScreen 
                isDarkTheme={isDarkTheme} 
                toggleTheme={toggleTheme} 
              />
            )}
          />
        </Tab.Navigator>
      </NavigationContainer>
    </PaperProvider>
  );
}
```

## Step 3: Creating the Home Screen

Create `screens/HomeScreen.js` to showcase basic Paper components:

```javascript
// Import React and React Native components
import React from 'react';
import { ScrollView, StyleSheet } from 'react-native';

// Import React Native Paper components
import {
  Card,
  Text,
  Button,
  Chip,
  Avatar,
  Surface,
  useTheme,
} from 'react-native-paper';

export default function HomeScreen() {
  // Access the current theme using useTheme hook
  const theme = useTheme();

  return (
    <ScrollView style={[styles.container, { backgroundColor: theme.colors.background }]}>
      {/* Welcome Card - demonstrates Card and Text components */}
      <Card style={styles.card}>
        <Card.Content>
          {/* Text component with titleLarge variant for headings */}
          <Text variant="titleLarge">Welcome to React Native Paper</Text>
          {/* Text component with bodyLarge variant for body text */}
          <Text variant="bodyLarge">
            React Native Paper is a collection of customizable and production-ready 
            components for React Native, following Google's Material Design guidelines.
          </Text>
        </Card.Content>
      </Card>

      {/* Features Card with Avatar */}
      <Card style={styles.card}>
        <Card.Title
          title="Material Design Components"
          subtitle="Beautiful, accessible UI components"
          // Avatar.Icon creates a circular icon background
          left={(props) => <Avatar.Icon {...props} icon="material-design" />}
        />
        <Card.Content>
          <Text variant="bodyLarge">
            This app demonstrates various React Native Paper components including 
            buttons, cards, forms, and theming capabilities.
          </Text>
        </Card.Content>
        <Card.Actions>
          {/* Button components with different modes */}
          <Button mode="text">Learn More</Button>
          <Button mode="contained">Get Started</Button>
        </Card.Actions>
      </Card>

      {/* Surface component for elevated content */}
      <Surface style={styles.surface} elevation={2}>
        <Text variant="titleLarge">Key Features</Text>
        
        {/* Container for feature chips */}
        <ScrollView 
          horizontal 
          showsHorizontalScrollIndicator={false} 
          style={styles.chipContainer}
        >
          {/* Chip components for tags/categories */}
          <Chip icon="palette" style={styles.chip}>Theming</Chip>
          <Chip icon="widgets" style={styles.chip}>Components</Chip>
          <Chip icon="responsive" style={styles.chip}>Responsive</Chip>
          <Chip icon="human" style={styles.chip}>Accessible</Chip>
          <Chip icon="material-design" style={styles.chip}>Material Design</Chip>
        </ScrollView>
      </Surface>

      {/* Quick Stats Card */}
      <Card style={styles.card}>
        <Card.Content>
          <Text variant="titleLarge">Library Stats</Text>
          <Text variant="bodyLarge">React Native Paper provides:</Text>
          
          {/* Using Surface for stat items */}
          <Surface style={styles.statItem} elevation={1}>
            <Text variant="titleLarge">50+</Text>
            <Text variant="bodyLarge">Components</Text>
          </Surface>
          
          <Surface style={styles.statItem} elevation={1}>
            <Text variant="titleLarge">100%</Text>
            <Text variant="bodyLarge">Material Design</Text>
          </Surface>
        </Card.Content>
      </Card>
    </ScrollView>
  );
}

// Styles for the Home Screen
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  card: {
    marginBottom: 16,
    // Card automatically applies elevation and rounded corners
  },
  surface: {
    padding: 16,
    marginBottom: 16,
    borderRadius: 8,
    // Surface provides elevation and background color from theme
  },
  chipContainer: {
    marginTop: 12,
  },
  chip: {
    marginRight: 8,
    // Chips automatically apply theme colors and spacing
  },
  statItem: {
    padding: 12,
    marginTop: 8,
    borderRadius: 6,
    alignItems: 'center',
  },
});
```

## Step 4: Testing Basic Setup

1. Start your Expo development server:

```bash
npx expo start
```

2. Testing Checklist:
   - Open the app on your device or simulator
   - Verify the Material Design styling is applied
   - Check that all tabs are visible with proper icons
   - Test the Home screen displays cards and components
   - Verify the theme colors are consistent throughout

3. Create a commit for your basic setup:
   - Open GitHub Desktop
   - Review your changes
   - Enter a commit message: "Initial React Native Paper setup with Home screen"
   - Click "Commit to main"
   - Click "Push origin"

## Step 5: Creating the Components Showcase Screen

Create `screens/ComponentsScreen.js` to demonstrate popular Paper components:

```javascript
// Import React hooks and React Native components
import React, { useState } from 'react';
import { ScrollView, StyleSheet, View } from 'react-native';

// Import various React Native Paper components
import {
  Card,
  Title,
  Paragraph,
  Button,
  IconButton,
  FAB,
  Badge,
  Avatar,
  Chip,
  ProgressBar,
  ActivityIndicator,
  Divider,
  List,
  Surface,
  useTheme,
  Snackbar,
  Portal,
} from 'react-native-paper';

export default function ComponentsScreen() {
  // Access current theme
  const theme = useTheme();
  
  // State for interactive components
  const [progress, setProgress] = useState(0.7); // Progress bar value
  const [expanded, setExpanded] = useState(false); // List accordion state
  const [snackbarVisible, setSnackbarVisible] = useState(false); // Snackbar visibility
  const [selectedChips, setSelectedChips] = useState(new Set()); // Selected chips

  // Function to toggle chip selection
  const toggleChip = (chipId) => {
    const newSelected = new Set(selectedChips);
    if (newSelected.has(chipId)) {
      newSelected.delete(chipId);
    } else {
      newSelected.add(chipId);
    }
    setSelectedChips(newSelected);
  };

  return (
    <View style={{ flex: 1 }}>
      <ScrollView style={[styles.container, { backgroundColor: theme.colors.background }]}>
        
        {/* Button Variants Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Title>Button Components</Title>
            <Paragraph>Different button styles and states:</Paragraph>
            
            {/* Container for button examples */}
            <View style={styles.buttonContainer}>
              {/* Contained button - most prominent */}
              <Button mode="contained" onPress={() => setSnackbarVisible(true)}>
                Contained
              </Button>
              
              {/* Outlined button - medium emphasis */}
              <Button mode="outlined" onPress={() => setSnackbarVisible(true)}>
                Outlined
              </Button>
              
              {/* Text button - low emphasis */}
              <Button mode="text" onPress={() => setSnackbarVisible(true)}>
                Text
              </Button>
              
              {/* Button with icon */}
              <Button 
                mode="contained" 
                icon="heart" 
                onPress={() => setSnackbarVisible(true)}
              >
                With Icon
              </Button>
              
              {/* Disabled button */}
              <Button mode="contained" disabled>
                Disabled
              </Button>
            </View>
          </Card.Content>
        </Card>

        {/* Icon Buttons and FAB Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Title>Icon Buttons & FAB</Title>
            <Paragraph>Circular action buttons:</Paragraph>
            
            <View style={styles.iconButtonContainer}>
              {/* Standard icon button */}
              <IconButton 
                icon="heart" 
                size={24} 
                onPress={() => setSnackbarVisible(true)} 
              />
              
              {/* Icon button with background */}
              <IconButton 
                icon="star" 
                mode="contained"
                size={24} 
                onPress={() => setSnackbarVisible(true)} 
              />
              
              {/* Icon button with outline */}
              <IconButton 
                icon="bookmark" 
                mode="outlined"
                size={24} 
                onPress={() => setSnackbarVisible(true)} 
              />
            </View>
            
            {/* Floating Action Button - positioned relative to card */}
            <View style={styles.fabContainer}>
              <FAB
                icon="plus"
                size="small"
                onPress={() => setSnackbarVisible(true)}
              />
              <FAB
                icon="pencil"
                size="medium"
                style={styles.fab}
                onPress={() => setSnackbarVisible(true)}
              />
            </View>
          </Card.Content>
        </Card>

        {/* Avatars and Badges Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Title>Avatars & Badges</Title>
            <Paragraph>User representations and notifications:</Paragraph>
            
            <View style={styles.avatarContainer}>
              {/* Text avatar */}
              <View style={styles.avatarItem}>
                <Avatar.Text size={48} label="JD" />
                <Badge style={styles.badge}>3</Badge>
              </View>
              
              {/* Icon avatar */}
              <View style={styles.avatarItem}>
                <Avatar.Icon size={48} icon="account" />
                <Badge style={styles.badge}>5</Badge>
              </View>
              
              {/* Image avatar (placeholder) */}
              <View style={styles.avatarItem}>
                <Avatar.Image 
                  size={48} 
                  source={{ uri: 'https://picsum.photos/100' }} 
                />
                <Badge style={styles.badge} visible={true}>New</Badge>
              </View>
            </View>
          </Card.Content>
        </Card>

        {/* Chips Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Title>Chips</Title>
            <Paragraph>Selectable tags and filters:</Paragraph>
            
            <View style={styles.chipContainer}>
              {/* Filter chips that can be toggled */}
              {['React', 'JavaScript', 'Mobile', 'Design', 'UI/UX'].map((label, index) => (
                <Chip
                  key={index}
                  selected={selectedChips.has(index)}
                  onPress={() => toggleChip(index)}
                  style={styles.chip}
                  icon={selectedChips.has(index) ? 'check' : 'plus'}
                >
                  {label}
                </Chip>
              ))}
            </View>
          </Card.Content>
        </Card>

        {/* Progress Indicators Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Title>Progress Indicators</Title>
            <Paragraph>Loading states and progress tracking:</Paragraph>
            
            {/* Progress bar with current value */}
            <View style={styles.progressContainer}>
              <Paragraph>Download Progress: {Math.round(progress * 100)}%</Paragraph>
              <ProgressBar progress={progress} color={theme.colors.primary} />
              
              {/* Buttons to control progress */}
              <View style={styles.progressButtons}>
                <Button 
                  mode="outlined" 
                  onPress={() => setProgress(Math.max(0, progress - 0.2))}
                >
                  Decrease
                </Button>
                <Button 
                  mode="outlined" 
                  onPress={() => setProgress(Math.min(1, progress + 0.2))}
                >
                  Increase
                </Button>
              </View>
            </View>
            
            <Divider style={styles.divider} />
            
            {/* Activity indicators */}
            <View style={styles.activityContainer}>
              <Paragraph>Loading indicators:</Paragraph>
              <ActivityIndicator animating={true} size="small" />
              <ActivityIndicator animating={true} size="large" />
            </View>
          </Card.Content>
        </Card>

        {/* List Components Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Title>List Components</Title>
            <Paragraph>Structured content lists:</Paragraph>
          </Card.Content>
          
          {/* List with various item types */}
          <List.Section>
            {/* Basic list item */}
            <List.Item
              title="Basic List Item"
              description="Simple item with title and description"
              left={props => <List.Icon {...props} icon="folder" />}
              right={props => <List.Icon {...props} icon="chevron-right" />}
            />
            
            {/* Accordion list item */}
            <List.Accordion
              title="Expandable Section"
              left={props => <List.Icon {...props} icon="folder-open" />}
              expanded={expanded}
              onPress={() => setExpanded(!expanded)}
            >
              <List.Item title="Nested Item 1" left={props => <List.Icon {...props} icon="file" />} />
              <List.Item title="Nested Item 2" left={props => <List.Icon {...props} icon="file" />} />
            </List.Accordion>
            
            {/* List item with avatar */}
            <List.Item
              title="User Profile"
              description="View profile settings"
              left={props => <Avatar.Icon size={40} icon="account" />}
              right={props => <List.Icon {...props} icon="chevron-right" />}
            />
          </List.Section>
        </Card>
      </ScrollView>

      {/* Portal for overlays like Snackbar */}
      <Portal>
        <Snackbar
          visible={snackbarVisible}
          onDismiss={() => setSnackbarVisible(false)}
          duration={2000}
          action={{
            label: 'Undo',
            onPress: () => {
              // Handle undo action
            },
          }}
        >
          Component interaction detected!
        </Snackbar>
      </Portal>
    </View>
  );
}

// Styles for the Components Screen
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  card: {
    marginBottom: 16,
  },
  buttonContainer: {
    marginTop: 12,
    gap: 8, // Space between buttons
  },
  iconButtonContainer: {
    flexDirection: 'row',
    marginTop: 12,
    gap: 8,
  },
  fabContainer: {
    flexDirection: 'row',
    marginTop: 16,
    gap: 12,
    alignItems: 'center',
  },
  fab: {
    marginLeft: 8,
  },
  avatarContainer: {
    flexDirection: 'row',
    marginTop: 12,
    gap: 20,
  },
  avatarItem: {
    position: 'relative',
    alignItems: 'center',
  },
  badge: {
    position: 'absolute',
    top: -8,
    right: -8,
  },
  chipContainer: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    marginTop: 12,
    gap: 8,
  },
  chip: {
    marginBottom: 4,
  },
  progressContainer: {
    marginTop: 12,
  },
  progressButtons: {
    flexDirection: 'row',
    marginTop: 12,
    gap: 8,
  },
  divider: {
    marginVertical: 16,
  },
  activityContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 16,
  },
});
```

## Step 6: Testing the Components Screen

1. Test the Components Screen:
   - Navigate to the Components tab
   - Try interacting with different buttons
   - Test the chip selection functionality
   - Expand/collapse the accordion list
   - Adjust the progress bar using the buttons
   - Verify the snackbar appears when buttons are pressed

2. Create a commit for the Components Screen:
   - Review your changes in GitHub Desktop
   - Enter commit message: "Added comprehensive Components showcase screen"
   - Click "Commit to main"
   - Click "Push origin"

## Step 7: Creating the Forms Screen

Create `screens/FormsScreen.js` to showcase form components and input handling:

```javascript
// Import React hooks and React Native components
import React, { useState } from 'react';
import { ScrollView, StyleSheet, View } from 'react-native';

// Import React Native Paper form and input components
import {
  Card,
  Text,
  TextInput,
  Button,
  Switch,
  RadioButton,
  Checkbox,
  HelperText,
  Surface,
  useTheme,
  Snackbar,
  Portal,
  Menu,
  Divider,
} from 'react-native-paper';

export default function FormsScreen({ isDarkTheme, toggleTheme }) {
  // Access current theme
  const theme = useTheme();
  
  // Form state management
  const [formData, setFormData] = useState({
    email: '',
    password: '',
    confirmPassword: '',
    name: '',
    bio: '',
    newsletter: false,
    notifications: true,
    theme: 'auto',
    country: '',
  });
  
  // UI state
  const [showPassword, setShowPassword] = useState(false);
  const [showConfirmPassword, setShowConfirmPassword] = useState(false);
  const [snackbarVisible, setSnackbarVisible] = useState(false);
  const [snackbarMessage, setSnackbarMessage] = useState('');
  const [menuVisible, setMenuVisible] = useState(false);
  const [selectedHobbies, setSelectedHobbies] = useState(new Set());

  // Form validation helpers
  const isEmailValid = (email) => {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  };

  const isPasswordValid = (password) => {
    return password.length >= 6;
  };

  const doPasswordsMatch = () => {
    return formData.password === formData.confirmPassword;
  };

  // Handle form input changes
  const updateFormData = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
  };

  // Handle hobby checkbox changes
  const toggleHobby = (hobby) => {
    const newHobbies = new Set(selectedHobbies);
    if (newHobbies.has(hobby)) {
      newHobbies.delete(hobby);
    } else {
      newHobbies.add(hobby);
    }
    setSelectedHobbies(newHobbies);
  };

  // Handle form submission
  const handleSubmit = () => {
    if (!isEmailValid(formData.email)) {
      setSnackbarMessage('Please enter a valid email address');
      setSnackbarVisible(true);
      return;
    }
    
    if (!isPasswordValid(formData.password)) {
      setSnackbarMessage('Password must be at least 6 characters');
      setSnackbarVisible(true);
      return;
    }
    
    if (!doPasswordsMatch()) {
      setSnackbarMessage('Passwords do not match');
      setSnackbarVisible(true);
      return;
    }
    
    setSnackbarMessage('Form submitted successfully!');
    setSnackbarVisible(true);
  };

  return (
    <View style={{ flex: 1 }}>
      <ScrollView style={[styles.container, { backgroundColor: theme.colors.background }]}>
        
        {/* Basic Text Inputs Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Text variant="titleLarge">Text Input Components</Text>
            <Text variant="bodyMedium">Various input field styles and validation:</Text>
            
            {/* Email input with validation */}
            <TextInput
              label="Email Address"
              value={formData.email}
              onChangeText={(text) => updateFormData('email', text)}
              mode="outlined"
              keyboardType="email-address"
              autoCapitalize="none"
              style={styles.input}
              error={formData.email.length > 0 && !isEmailValid(formData.email)}
              left={<TextInput.Icon icon="email" />}
            />
            {/* Helper text for validation feedback */}
            <HelperText type="error" visible={formData.email.length > 0 && !isEmailValid(formData.email)}>
              Please enter a valid email address
            </HelperText>
            
            {/* Full name input */}
            <TextInput
              label="Full Name"
              value={formData.name}
              onChangeText={(text) => updateFormData('name', text)}
              mode="outlined"
              style={styles.input}
              left={<TextInput.Icon icon="account" />}
            />
            
            {/* Password input with visibility toggle */}
            <TextInput
              label="Password"
              value={formData.password}
              onChangeText={(text) => updateFormData('password', text)}
              mode="outlined"
              secureTextEntry={!showPassword}
              style={styles.input}
              error={formData.password.length > 0 && !isPasswordValid(formData.password)}
              left={<TextInput.Icon icon="lock" />}
              right={
                <TextInput.Icon
                  icon={showPassword ? "eye-off" : "eye"}
                  onPress={() => setShowPassword(!showPassword)}
                />
              }
            />
            <HelperText type="error" visible={formData.password.length > 0 && !isPasswordValid(formData.password)}>
              Password must be at least 6 characters
            </HelperText>
            
            {/* Confirm password input */}
            <TextInput
              label="Confirm Password"
              value={formData.confirmPassword}
              onChangeText={(text) => updateFormData('confirmPassword', text)}
              mode="outlined"
              secureTextEntry={!showConfirmPassword}
              style={styles.input}
              error={formData.confirmPassword.length > 0 && !doPasswordsMatch()}
              left={<TextInput.Icon icon="lock-check" />}
              right={
                <TextInput.Icon
                  icon={showConfirmPassword ? "eye-off" : "eye"}
                  onPress={() => setShowConfirmPassword(!showConfirmPassword)}
                />
              }
            />
            <HelperText type="error" visible={formData.confirmPassword.length > 0 && !doPasswordsMatch()}>
              Passwords do not match
            </HelperText>
          </Card.Content>
        </Card>

        {/* Multiline Text Input Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Text variant="titleLarge">Multiline Input</Text>
            <Text variant="bodyMedium">Text areas for longer content:</Text>
            
            {/* Bio text area */}
            <TextInput
              label="Bio"
              value={formData.bio}
              onChangeText={(text) => updateFormData('bio', text)}
              mode="outlined"
              multiline
              numberOfLines={4}
              style={styles.input}
              placeholder="Tell us about yourself..."
              left={<TextInput.Icon icon="account-edit" />}
            />
            <HelperText type="info">
              {formData.bio.length}/200 characters
            </HelperText>
          </Card.Content>
        </Card>

        {/* Selection Components Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Text variant="titleLarge">Selection Components</Text>
            <Text variant="bodyMedium">Switches, radio buttons, and checkboxes:</Text>
            
            {/* Switch components */}
            <Surface style={styles.selectionItem} elevation={1}>
              <View style={styles.selectionRow}>
                <View style={styles.selectionText}>
                  <Text variant="bodyMedium">Newsletter Subscription</Text>
                  <HelperText type="info">
                    Receive updates about new features
                  </HelperText>
                </View>
                <Switch
                  value={formData.newsletter}
                  onValueChange={(value) => updateFormData('newsletter', value)}
                />
              </View>
            </Surface>
            
            <Surface style={styles.selectionItem} elevation={1}>
              <View style={styles.selectionRow}>
                <View style={styles.selectionText}>
                  <Text variant="bodyMedium">Push Notifications</Text>
                  <HelperText type="info">
                    Get notified about important updates
                  </HelperText>
                </View>
                <Switch
                  value={formData.notifications}
                  onValueChange={(value) => updateFormData('notifications', value)}
                />
              </View>
            </Surface>

            <Surface style={styles.selectionItem} elevation={1}>
              <View style={styles.selectionRow}>
                <View style={styles.selectionText}>
                  <Text variant="bodyMedium">Dark Mode</Text>
                  <HelperText type="info">
                    Toggle between light and dark themes
                  </HelperText>
                </View>
                <Switch
                  value={isDarkTheme}
                  onValueChange={toggleTheme}
                />
              </View>
            </Surface>

            <Divider style={styles.divider} />

            {/* Radio Button Group */}
            <View style={styles.radioGroup}>
              <Text variant="titleMedium" style={styles.groupTitle}>Theme Preference</Text>
              <RadioButton.Group
                onValueChange={(value) => updateFormData('theme', value)}
                value={formData.theme}
              >
                <RadioButton.Item
                  label="Light"
                  value="light"
                  labelStyle={styles.radioLabel}
                />
                <RadioButton.Item
                  label="Dark"
                  value="dark"
                  labelStyle={styles.radioLabel}
                />
                <RadioButton.Item
                  label="System Default"
                  value="auto"
                  labelStyle={styles.radioLabel}
                />
              </RadioButton.Group>
            </View>

            <Divider style={styles.divider} />

            {/* Checkbox Group */}
            <View style={styles.checkboxGroup}>
              <Text variant="titleMedium" style={styles.groupTitle}>Interests & Hobbies</Text>
              <View style={styles.checkboxGrid}>
                {['Reading', 'Gaming', 'Sports', 'Music', 'Travel', 'Cooking'].map((hobby) => (
                  <View key={hobby} style={styles.checkboxItem}>
                    <Checkbox
                      status={selectedHobbies.has(hobby) ? 'checked' : 'unchecked'}
                      onPress={() => toggleHobby(hobby)}
                    />
                    <Text variant="bodyMedium" style={styles.checkboxLabel}>{hobby}</Text>
                  </View>
                ))}
              </View>
            </View>
          </Card.Content>
        </Card>

        {/* Menu Selection Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Text variant="titleLarge">Menu Selection</Text>
            <Text variant="bodyMedium">Dropdown menu for country selection:</Text>
            
            <Surface style={styles.menuSurface} elevation={1}>
              <Menu
                visible={menuVisible}
                onDismiss={() => setMenuVisible(false)}
                anchor={
                  <Button
                    mode="outlined"
                    onPress={() => setMenuVisible(true)}
                    style={styles.menuButton}
                  >
                    {formData.country || 'Select Country'}
                  </Button>
                }
              >
                <Menu.Item
                  onPress={() => {
                    updateFormData('country', 'United States');
                    setMenuVisible(false);
                  }}
                  title="United States"
                />
                <Menu.Item
                  onPress={() => {
                    updateFormData('country', 'Canada');
                    setMenuVisible(false);
                  }}
                  title="Canada"
                />
                <Menu.Item
                  onPress={() => {
                    updateFormData('country', 'United Kingdom');
                    setMenuVisible(false);
                  }}
                  title="United Kingdom"
                />
                <Menu.Item
                  onPress={() => {
                    updateFormData('country', 'Australia');
                    setMenuVisible(false);
                  }}
                  title="Australia"
                />
                <Divider />
                <Menu.Item
                  onPress={() => {
                    updateFormData('country', '');
                    setMenuVisible(false);
                  }}
                  title="Clear Selection"
                />
              </Menu>
            </Surface>
          </Card.Content>
        </Card>

        {/* Form Actions Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Text variant="titleLarge">Form Actions</Text>
            <Text variant="bodyMedium">Submit or reset the form:</Text>
            
            <View style={styles.actionButtons}>
              <Button
                mode="contained"
                onPress={handleSubmit}
                style={styles.submitButton}
              >
                Submit
              </Button>
              <Button
                mode="outlined"
                onPress={() => {
                  setFormData({
                    email: '',
                    password: '',
                    confirmPassword: '',
                    name: '',
                    bio: '',
                    newsletter: false,
                    notifications: true,
                    theme: 'auto',
                    country: '',
                  });
                  setSelectedHobbies(new Set());
                }}
              >
                Reset
              </Button>
            </View>
          </Card.Content>
        </Card>

        {/* Form Summary Card */}
        <Card style={styles.card}>
          <Card.Content>
            <Text variant="titleLarge">Form Summary</Text>
            <Text variant="bodyMedium">Current form values:</Text>
            
            <Surface style={styles.summarySurface} elevation={1}>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Email:</Text>
                <Text variant="bodyMedium">{formData.email || 'Not set'}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Name:</Text>
                <Text variant="bodyMedium">{formData.name || 'Not set'}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Bio:</Text>
                <Text variant="bodyMedium">{formData.bio || 'Not set'}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Newsletter:</Text>
                <Text variant="bodyMedium">{formData.newsletter ? 'Subscribed' : 'Not subscribed'}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Notifications:</Text>
                <Text variant="bodyMedium">{formData.notifications ? 'Enabled' : 'Disabled'}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Theme:</Text>
                <Text variant="bodyMedium">{formData.theme}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Country:</Text>
                <Text variant="bodyMedium">{formData.country || 'Not selected'}</Text>
              </View>
              <View style={styles.summaryRow}>
                <Text variant="titleMedium">Hobbies:</Text>
                <Text variant="bodyMedium">
                  {Array.from(selectedHobbies).length > 0
                    ? Array.from(selectedHobbies).join(', ')
                    : 'None selected'}
                </Text>
              </View>
            </Surface>
          </Card.Content>
        </Card>
      </ScrollView>

      {/* Snackbar for form feedback */}
      <Snackbar
        visible={snackbarVisible}
        onDismiss={() => setSnackbarVisible(false)}
        duration={3000}
        action={{
          label: 'OK',
          onPress: () => setSnackbarVisible(false),
        }}
      >
        {snackbarMessage}
      </Snackbar>
    </View>
  );
}

// Styles for the Forms Screen
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  card: {
    marginBottom: 16,
  },
  input: {
    marginBottom: 8,
  },
  selectionItem: {
    marginVertical: 8,
    padding: 16,
    borderRadius: 8,
  },
  selectionRow: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
  },
  selectionText: {
    flex: 1,
  },
  divider: {
    marginVertical: 16,
  },
  radioGroup: {
    marginVertical: 8,
  },
  groupTitle: {
    marginBottom: 8,
  },
  radioLabel: {
    fontSize: 16,
  },
  checkboxGroup: {
    marginVertical: 8,
  },
  checkboxGrid: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    marginTop: 8,
  },
  checkboxItem: {
    flexDirection: 'row',
    alignItems: 'center',
    width: '50%',
    marginBottom: 8,
  },
  checkboxLabel: {
    marginLeft: 8,
  },
  menuSurface: {
    marginTop: 16,
    borderRadius: 8,
  },
  menuButton: {
    width: '100%',
  },
  actionButtons: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginTop: 16,
  },
  submitButton: {
    flex: 1,
    marginRight: 8,
  },
  summarySurface: {
    marginTop: 16,
    padding: 16,
    borderRadius: 8,
  },
  summaryRow: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 8,
  },
});
```

## Step 8: Creating the Theme Customization Screen

Create `screens/ThemeScreen.js` to demonstrate theming capabilities:

```javascript
// Import React hooks and React Native components
import React, { useState } from 'react';
import { ScrollView, StyleSheet, View } from 'react-native';

// Import React Native Paper components and theming utilities
import {
  Card,
  Title,
  Paragraph,
  Button,
  Switch,
  Surface,
  useTheme,
  Chip,
  Avatar,
  List,
  Divider,
  IconButton,
} from 'react-native-paper';

// Component receives theme props from App.js
export default function ThemeScreen({ isDarkTheme, toggleTheme }) {
  // Access current theme object
  const theme = useTheme();
  
  // State for theme preview features
  const [showColorPalette, setShowColorPalette] = useState(true);

  // Function to display color values in a readable format
  const formatColor = (color) => {
    return color?.toUpperCase() || 'Not defined';
  };

  return (
    <ScrollView style={[styles.container, { backgroundColor: theme.colors.background }]}>
      
      {/* Theme Toggle Card */}
      <Card style={styles.card}>
        <Card.Content>
          <Title>Theme Settings</Title>
          <Paragraph>
            React Native Paper supports Material Design 3 theming with automatic 
            light and dark mode support.
          </Paragraph>
          
          {/* Theme toggle switch */}
          <Surface style={styles.themeToggle} elevation={1}>
            <View style={styles.toggleRow}>
              <View style={styles.toggleText}>
                <Paragraph style={styles.toggleTitle}>Dark Mode</Paragraph>
                <Paragraph style={styles.toggleSubtitle}>
                  {isDarkTheme ? 'Dark theme active' : 'Light theme active'}
                </Paragraph>
              </View>
              <Switch
                value={isDarkTheme}
                onValueChange={toggleTheme}
              />
            </View>
          </Surface>
        </Card.Content>
      </Card>

      {/* Current Theme Info Card */}
      <Card style={styles.card}>
        <Card.Content>
          <Title>Current Theme Information</Title>
          <Paragraph>Theme version and configuration details:</Paragraph>
          
          <Surface style={styles.infoSurface} elevation={1}>
            <Paragraph><strong>Theme Version:</strong> Material Design 3</Paragraph>
            <Paragraph><strong>Mode:</strong> {isDarkTheme ? 'Dark' : 'Light'}</Paragraph>
            <Paragraph><strong>Round Factor:</strong> {theme.roundness}px</Paragraph>
            <Paragraph><strong>Animation Scale:</strong> {theme.animation.scale}</Paragraph>
          </Surface>
        </Card.Content>
      </Card>

      {/* Color Palette Card */}
      <Card style={styles.card}>
        <Card.Title
          title="Color Palette"
          subtitle="Material Design 3 color system"
          right={(props) => (
            <IconButton
              {...props}
              icon={showColorPalette ? 'chevron-up' : 'chevron-down'}
              onPress={() => setShowColorPalette(!showColorPalette)}
            />
          )}
        />
        
        {showColorPalette && (
          <Card.Content>
            <Paragraph style={styles.paletteDescription}>
              The Material Design 3 color system includes semantic color roles 
              that adapt automatically between light and dark themes.
            </Paragraph>
            
            {/* Primary Colors */}
            <View style={styles.colorSection}>
              <Paragraph style={styles.sectionTitle}>Primary Colors</Paragraph>
              <View style={styles.colorRow}>
                <Surface 
                  style={[styles.colorSwatch, { backgroundColor: theme.colors.primary }]} 
                  elevation={2}
                >
                  <Paragraph style={[styles.colorLabel, { color: theme.colors.onPrimary }]}>
                    Primary
                  </Paragraph>
                </Surface>
                <View style={styles.colorInfo}>
                  <Paragraph style={styles.colorName}>Primary</Paragraph>
                  <Paragraph style={styles.colorValue}>{formatColor(theme.colors.primary)}</Paragraph>
                </View>
              </View>
              
              <View style={styles.colorRow}>
                <Surface 
                  style={[styles.colorSwatch, { backgroundColor: theme.colors.primaryContainer }]} 
                  elevation={2}
                >
                  <Paragraph style={[styles.colorLabel, { color: theme.colors.onPrimaryContainer }]}>
                    Container
                  </Paragraph>
                </Surface>
                <View style={styles.colorInfo}>
                  <Paragraph style={styles.colorName}>Primary Container</Paragraph>
                  <Paragraph style={styles.colorValue}>{formatColor(theme.colors.primaryContainer)}</Paragraph>
                </View>
              </View>
            </View>

            {/* Secondary Colors */}
            <View style={styles.colorSection}>
              <Paragraph style={styles.sectionTitle}>Secondary Colors</Paragraph>
              <View style={styles.colorRow}>
                <Surface 
                  style={[styles.colorSwatch, { backgroundColor: theme.colors.secondary }]} 
                  elevation={2}
                >
                  <Paragraph style={[styles.colorLabel, { color: theme.colors.onSecondary }]}>
                    Secondary
                  </Paragraph>
                </Surface>
                <View style={styles.colorInfo}>
                  <Paragraph style={styles.colorName}>Secondary</Paragraph>
                  <Paragraph style={styles.colorValue}>{formatColor(theme.colors.secondary)}</Paragraph>
                </View>
              </View>
            </View>

            {/* Surface Colors */}
            <View style={styles.colorSection}>
              <Paragraph style={styles.sectionTitle}>Surface Colors</Paragraph>
              <View style={styles.colorRow}>
                <Surface 
                  style={[styles.colorSwatch, { backgroundColor: theme.colors.surface }]} 
                  elevation={2}
                >
                  <Paragraph style={[styles.colorLabel, { color: theme.colors.onSurface }]}>
                    Surface
                  </Paragraph>
                </Surface>
                <View style={styles.colorInfo}>
                  <Paragraph style={styles.colorName}>Surface</Paragraph>
                  <Paragraph style={styles.colorValue}>{formatColor(theme.colors.surface)}</Paragraph>
                </View>
              </View>
              
              <View style={styles.colorRow}>
                <Surface 
                  style={[styles.colorSwatch, { backgroundColor: theme.colors.surfaceVariant }]} 
                  elevation={2}
                >
                  <Paragraph style={[styles.colorLabel, { color: theme.colors.onSurfaceVariant }]}>
                    Variant
                  </Paragraph>
                </Surface>
                <View style={styles.colorInfo}>
                  <Paragraph style={styles.colorName}>Surface Variant</Paragraph>
                  <Paragraph style={styles.colorValue}>{formatColor(theme.colors.surfaceVariant)}</Paragraph>
                </View>
              </View>
            </View>

            {/* Semantic Colors */}
            <View style={styles.colorSection}>
              <Paragraph style={styles.sectionTitle}>Semantic Colors</Paragraph>
              <View style={styles.colorRow}>
                <Surface 
                  style={[styles.colorSwatch, { backgroundColor: theme.colors.error }]} 
                  elevation={2}
                >
                  <Paragraph style={[styles.colorLabel, { color: theme.colors.onError }]}>
                    Error
                  </Paragraph>
                </Surface>
                <View style={styles.colorInfo}>
                  <Paragraph style={styles.colorName}>Error</Paragraph>
                  <Paragraph style={styles.colorValue}>{formatColor(theme.colors.error)}</Paragraph>
                </View>
              </View>
            </View>
          </Card.Content>
        )}
      </Card>

      {/* Typography Card */}
      <Card style={styles.card}>
        <Card.Content>
          <Title>Typography Scale</Title>
          <Paragraph>Material Design 3 typography system:</Paragraph>
          
          <Surface style={styles.typographyContainer} elevation={1}>
            {/* Display different text styles */}
            <View style={styles.textExample}>
              <Title style={theme.fonts.displayLarge}>Display Large</Title>
              <Paragraph style={styles.typeInfo}>Display Large - Headlines</Paragraph>
            </View>
            
            <Divider style={styles.textDivider} />
            
            <View style={styles.textExample}>
              <Title style={theme.fonts.headlineMedium}>Headline Medium</Title>
              <Paragraph style={styles.typeInfo}>Headline Medium - Section headers</Paragraph>
            </View>
            
            <Divider style={styles.textDivider} />
            
            <View style={styles.textExample}>
              <Title style={theme.fonts.titleLarge}>Title Large</Title>
              <Paragraph style={styles.typeInfo}>Title Large - Card titles</Paragraph>
            </View>
            
            <Divider style={styles.textDivider} />
            
            <View style={styles.textExample}>
              <Paragraph style={theme.fonts.bodyLarge}>Body Large - Primary text content</Paragraph>
              <Paragraph style={styles.typeInfo}>Body Large - Main content</Paragraph>
            </View>
            
            <Divider style={styles.textDivider} />
            
            <View style={styles.textExample}>
              <Paragraph style={theme.fonts.bodyMedium}>Body Medium - Secondary text content</Paragraph>
              <Paragraph style={styles.typeInfo}>Body Medium - Supporting text</Paragraph>
            </View>
            
            <Divider style={styles.textDivider} />
            
            <View style={styles.textExample}>
              <Paragraph style={theme.fonts.labelLarge}>Label Large - Button text</Paragraph>
              <Paragraph style={styles.typeInfo}>Label Large - Buttons and actions</Paragraph>
            </View>
          </Surface>
        </Card.Content>
      </Card>

      {/* Component Preview Card */}
      <Card style={styles.card}>
        <Card.Content>
          <Title>Component Preview</Title>
          <Paragraph>See how components look in the current theme:</Paragraph>
          
          {/* Preview different component states */}
          <View style={styles.previewSection}>
            <Paragraph style={styles.previewTitle}>Buttons</Paragraph>
            <View style={styles.buttonPreview}>
              <Button mode="contained">Contained</Button>
              <Button mode="outlined">Outlined</Button>
              <Button mode="text">Text</Button>
            </View>
          </View>
          
          <Divider style={styles.previewDivider} />
          
          <View style={styles.previewSection}>
            <Paragraph style={styles.previewTitle}>Chips</Paragraph>
            <View style={styles.chipPreview}>
              <Chip icon="star">Default</Chip>
              <Chip icon="heart" selected>Selected</Chip>
              <Chip icon="bookmark" disabled>Disabled</Chip>
            </View>
          </View>
          
          <Divider style={styles.previewDivider} />
          
          <View style={styles.previewSection}>
            <Paragraph style={styles.previewTitle}>List Items</Paragraph>
            <List.Item
              title="Theme Preview Item"
              description="This shows how list items appear"
              left={props => <Avatar.Icon {...props} size={40} icon="palette" />}
              right={props => <List.Icon {...props} icon="chevron-right" />}
            />
          </View>
        </Card.Content>
      </Card>

      {/* Theme Tips Card */}
      <Card style={styles.card}>
        <Card.Content>
          <Title>Theming Best Practices</Title>
          <Paragraph>Tips for effective Material Design theming:</Paragraph>
          
          <List.Section>
            <List.Item
              title="Use Semantic Colors"
              description="Always use theme.colors.primary instead of hardcoded colors"
              left={props => <List.Icon {...props} icon="palette" />}
            />
            <List.Item
              title="Respect User Preferences"
              description="Support both light and dark themes automatically"
              left={props => <List.Icon {...props} icon="theme-light-dark" />}
            />
            <List.Item
              title="Test Accessibility"
              description="Ensure sufficient contrast ratios in both themes"
              left={props => <List.Icon {...props} icon="accessibility" />}
            />
            <List.Item
              title="Consistent Typography"
              description="Use theme.fonts for consistent text styling"
              left={props => <List.Icon {...props} icon="format-text" />}
            />
          </List.Section>
        </Card.Content>
      </Card>
    </ScrollView>
  );
}

// Styles for the Theme Screen
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  card: {
    marginBottom: 16,
  },
  themeToggle: {
    padding: 16,
    marginTop: 12,
    borderRadius: 12,
  },
  toggleRow: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
  },
  toggleText: {
    flex: 1,
  },
  toggleTitle: {
    fontWeight: 'bold',
  },
  toggleSubtitle: {
    opacity: 0.7,
  },
  infoSurface: {
    padding: 12,
    marginTop: 8,
    borderRadius: 8,
  },
  paletteDescription: {
    marginBottom: 16,
    fontStyle: 'italic',
  },
  colorSection: {
    marginVertical: 12,
  },
  sectionTitle: {
    fontWeight: 'bold',
    marginBottom: 8,
  },
  colorRow: {
    flexDirection: 'row',
    alignItems: 'center',
    marginVertical: 6,
  },
  colorSwatch: {
    width: 60,
    height: 40,
    borderRadius: 8,
    justifyContent: 'center',
    alignItems: 'center',
    marginRight: 12,
  },
  colorLabel: {
    fontSize: 10,
    fontWeight: 'bold',
  },
  colorInfo: {
    flex: 1,
  },
  colorName: {
    fontWeight: '500',
  },
  colorValue: {
    fontSize: 12,
    opacity: 0.7,
    fontFamily: 'monospace',
  },
  typographyContainer: {
    padding: 16,
    marginTop: 8,
    borderRadius: 12,
  },
  textExample: {
    marginVertical: 8,
  },
  typeInfo: {
    fontSize: 12,
    opacity: 0.7,
    marginTop: 4,
  },
  textDivider: {
    marginVertical: 8,
  },
  previewSection: {
    marginVertical: 12,
  },
  previewTitle: {
    fontWeight: 'bold',
    marginBottom: 8,
  },
  buttonPreview: {
    flexDirection: 'row',
    gap: 8,
    flexWrap: 'wrap',
  },
  chipPreview: {
    flexDirection: 'row',
    gap: 8,
    flexWrap: 'wrap',
  },
  previewDivider: {
    marginVertical: 12,
  },
});
```

## Step 9: Testing the Complete Application

1. Test all screens thoroughly:
   - **Home Screen**: Verify welcome content and basic Paper components
   - **Components Screen**: Test interactive elements, progress bars, chips, lists
   - **Forms Screen**: Try form validation, input types, selections
   - **Theme Screen**: Toggle between light/dark themes, examine color palette

2. Test theme persistence:
   - Toggle to dark mode
   - Close and restart the app
   - Verify the theme preference is remembered

3. Test navigation:
   - Ensure smooth transitions between tabs
   - Verify icons and labels are correct
   - Check that theme changes apply to navigation

## Step 10: Final Commit and Documentation

1. Create your final commit:
   - Review all your changes in GitHub Desktop
   - Enter commit message: "Complete Material Design app with React Native Paper - All screens implemented with theming"
   - Click "Commit to main"
   - Click "Push origin"

## Step 11: Understanding React Native Paper Concepts

### Core Concepts Learned:

**1. Provider Pattern:**
- `PaperProvider` wraps your entire app to provide theming context
- All Paper components automatically receive theme values
- Enables consistent styling across the application

**2. Material Design 3 Theming:**
- Semantic color system (primary, secondary, surface, error)
- Automatic light/dark mode support
- Typography scale with predefined styles
- Elevation system for depth and hierarchy

**3. Component Categories:**
- **Containment**: Cards, Surfaces for grouping content
- **Navigation**: Integrated with React Navigation
- **Input**: TextInput with built-in validation and icons
- **Selection**: Switches, Radio buttons, Checkboxes, Menus
- **Communication**: Snackbars, Progress indicators
- **Display**: Lists, Chips, Avatars, Badges

**4. Theming Best Practices:**
- Use `useTheme()` hook to access current theme
- Apply theme colors dynamically: `theme.colors.primary`
- Support both light and dark modes
- Persist user theme preferences


This comprehensive tutorial has demonstrated the power and flexibility of React Native Paper for building beautiful, accessible Material Design applications. The component library provides everything needed to create professional mobile apps that follow Google's design guidelines while maintaining excellent performance and user experience.
