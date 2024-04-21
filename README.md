# TechmFinalProject
This repository consists of all the code files , screenshots of the mobile application and also a video explaining our application

```
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import HomeScreen from './components/Homescreen';
import BookingScreen from './components/BookingScreen';
import ProfileScreen from './components/ProfileScreen';
import ExperienceDetailsScreen from './components/ExperienceDetailsScreen';
import ConfirmationScreen from './components/ConfirmationScreen';
import LandingScreen from './components/LandingScreen';

const Stack = createStackNavigator();
const Tab = createBottomTabNavigator();

function MainStackNavigator() {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Landing" component={LandingScreen} />
      <Stack.Screen name="ExperienceDetails" component={ExperienceDetailsScreen} />
      <Stack.Screen name="Booking" component={BookingScreen} />
      <Stack.Screen name="ConfirmationScreen" component={ConfirmationScreen} />
    </Stack.Navigator>
  );
}

const App = () => {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="MainStack" component={MainStackNavigator} options={{ title: 'Discover' }}/>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Profile" component={ProfileScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
};

export default App;
```


