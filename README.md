# TechmFinalProject
This repository consists of all the code files , screenshots of the mobile application and also a video explaining our application

# App.js file
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
#Homescreen File
```
import React from 'react';
import { View, FlatList, StyleSheet, TouchableOpacity, Text, Image } from 'react-native';

const experiences = [
  {
    id: '1',
    title: 'Cooking Class with a Local Chef',
    description: 'Learn to cook authentic local dishes from an experienced chef.',
    price: 50,
    imageUrl: 'https://www.google.com/url?sa=i&url=https%3A%2F%2Fstock.adobe.com%2Fsearch%3Fk%3D%2522cooking%2Bclass%2522&psig=AOvVaw0axJdIEJ1xqwpZl7eBICaX&ust=1713232292252000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCNDSvemNw4UDFQAAAAAdAAAAABAE',
  },
  {
    id: '2',
    title: 'Street Art Tour',
    description: 'Explore the hidden street art gems of the city with a local guide.',
    price: 30,
    imageUrl: 'https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.viator.com%2Ftours%2FJaco%2FStreet-Art-Tour-Jaco%2Fd4144-407372P1&psig=AOvVaw1_WiCV1vfYLF2J_QxsyR5S&ust=1713232329727000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCJjyx6WOw4UDFQAAAAAdAAAAABAE',
  },
  {
    id: '3',
    title: 'Craft Beer Tasting',
    description: 'Sample the best local craft beers with a knowledgeable beer aficionado.',
    price: 40,
    imageUrl: 'https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.istockphoto.com%2Fphoto%2Fselection-of-craft-beers-in-a-flight-ready-for-tasting-gm1220023915-357074243&psig=AOvVaw2OV5lTIl5rBXuL5gejTYJr&ust=1713232562083000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCLjvueWOw4UDFQAAAAAdAAAAABAE',
  },
];

const HomeScreen = ({ navigation }) => {
  const renderExperienceItem = ({ item }) => (
    <TouchableOpacity
      style={styles.experienceItem}
      onPress={() => navigation.navigate('ExperienceDetails', { experience: item })}
    >
      <View style={styles.experienceInfo}>
        <Text style={styles.experienceTitle}>{item.title}</Text>
        <Text style={styles.experienceDescription}>{item.description}</Text>
        <Text style={styles.experiencePrice}>${item.price}</Text>
      </View>
    </TouchableOpacity>
  );

  return (
    <View style={styles.container}>
      <FlatList
        data={experiences}
        keyExtractor={(item) => item.id}
        renderItem={renderExperienceItem}
        contentContainerStyle={styles.experienceList}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f2f2f2',
  },
  experienceList: {
    padding: 16,
  },
  experienceItem: {
    backgroundColor: '#fff',
    borderRadius: 8,
    marginBottom: 16,
    overflow: 'hidden',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    elevation: 2,
  },
  experienceImage: {
    height: 200,
    width: '100%',
  },
  experienceInfo: {
    padding: 16,
  },
  experienceTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 8,
  },
  experienceDescription: {
    fontSize: 16,
    marginBottom: 8,
  },
  experiencePrice: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#4CAF50',
  },
});

export default HomeScreen;
```
#Booking Screen File
```
import React, { useState } from 'react';
import { View, Text, StyleSheet, TouchableOpacity, ScrollView, Image } from 'react-native';
import DateTimePickerModal from 'react-native-modal-datetime-picker';
import { Picker } from '@react-native-picker/picker';
import { useNavigation } from '@react-navigation/native';

const BookingScreen = ({ route }) => {
  const navigation = useNavigation();
  const { experience } = route.params;
  const [date, setDate] = useState(new Date());
  const [guests, setGuests] = useState(1);
  const [showDatePicker, setShowDatePicker] = useState(false);

  const handleDatePicked = (pickedDate) => {
    setDate(pickedDate);
    setShowDatePicker(false);
  };

  const handleBooking = () => {
    console.log('Booking is handled');
    navigation.navigate('ConfirmationScreen', {
      experience: experience,
      date: date.toISOString(),
      guests: guests,
    });
  };

  return (
    <ScrollView style={styles.container}>
      <View style={styles.detailsContainer}>
        <Text style={styles.title}>{experience.title}</Text>
        <Text style={styles.description}>{experience.description}</Text>
        <Text style={styles.price}>${experience.price}</Text>
      </View>

      <View style={styles.bookingContainer}>
        <Text style={styles.sectionTitle}>Book This Experience</Text>

        <TouchableOpacity style={styles.datePicker} onPress={() => setShowDatePicker(true)}>
          <Text style={styles.dateText}>{date.toLocaleDateString()}</Text>
        </TouchableOpacity>
        {showDatePicker && (
          <DateTimePickerModal
            isVisible={showDatePicker}
            mode="date"
            onConfirm={handleDatePicked}
            onCancel={() => setShowDatePicker(false)}
          />
        )}

        <View style={styles.guestsInput}>
          <Text style={styles.label}>Number of Guests</Text>
          <Picker selectedValue={guests} onValueChange={(itemValue) => setGuests(itemValue)}>
            {Array.from({ length: 10 }, (_, i) => i + 1).map((count) => (
              <Picker.Item key={count} label={`${count}`} value={count} />
            ))}
          </Picker>
        </View>

        <TouchableOpacity style={styles.bookButton} onPress={handleBooking}>
          <Text style={styles.bookButtonText}>Book Now</Text>
        </TouchableOpacity>
      </View>
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f2f2f2',
  },
  detailsContainer: {
    padding: 16,
    backgroundColor: '#fff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    elevation: 2,
  },
  experienceImage: {
    height: 200,
    width: '100%',
    marginBottom: 16,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 8,
  },
  description: {
    fontSize: 16,
    marginBottom: 8,
  },
  price: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#4CAF50',
  },
  bookingContainer: {
    padding: 16,
    backgroundColor: '#fff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    elevation: 2,
    marginTop: 16,
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 16,
  },
  datePicker: {
    backgroundColor: '#f2f2f2',
    paddingVertical: 12,
    paddingHorizontal: 16,
    borderRadius: 8,
    marginBottom: 16,
  },
  dateText: {
    fontSize: 16,
  },
  guestsInput: {
    marginBottom: 16,
  },
  label: {
    fontSize: 16,
    marginBottom: 8,
  },
  bookButton: {
    backgroundColor: '#4CAF50',
    paddingVertical: 12,
    paddingHorizontal: 24,
    borderRadius: 8,
  },
  bookButtonText: {
    color: '#fff',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default BookingScreen;
```
#Experience Details Screen File
```
import React from 'react';
import { View, Text, StyleSheet, Image, ScrollView, TouchableOpacity } from 'react-native';

const ExperienceDetailsScreen = ({ route, navigation }) => {
  const { experience } = route.params;

  const handleBooking = () => {
    navigation.navigate('Booking', { experience });
  };

  return (
    <ScrollView style={styles.container}>
      <View style={styles.detailsContainer}>
        <Text style={styles.title}>{experience.title}</Text>
        <Text style={styles.description}>{experience.description}</Text>
        <Text style={styles.price}>${experience.price}</Text>
        <TouchableOpacity style={styles.bookButton} onPress={handleBooking}>
          <Text style={styles.bookButtonText}>Book Now</Text>
        </TouchableOpacity>
      </View>
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f2f2f2',
  },
  experienceImage: {
    height: 300,
    width: '100%',
  },
  detailsContainer: {
    padding: 16,
    backgroundColor: '#fff',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    elevation: 2,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 8,
  },
  description: {
    fontSize: 16,
    marginBottom: 16,
  },
  price: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#4CAF50',
    marginBottom: 16,
  },
  bookButton: {
    backgroundColor: '#4CAF50',
    paddingVertical: 12,
    paddingHorizontal: 24,
    borderRadius: 8,
  },
  bookButtonText: {
    color: '#fff',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default ExperienceDetailsScreen;
```
#Landing Screen File
```
import React from 'react';
import { View, Text, StyleSheet, TouchableOpacity, ImageBackground } from 'react-native';

const LandingScreen = ({ navigation }) => {
  const handleGetStarted = () => {
    navigation.navigate('Home');
  };

  return (
    <ImageBackground
      source={require('./images/landing-background.jpg')}
      style={styles.container}
      blurRadius={2}
    >
      <View style={styles.contentContainer}>
        <Text style={styles.title}>Discover Unique Local Experiences</Text>
        <Text style={styles.subtitle}>
          Connect with locals and explore the hidden gems of every city.
        </Text>
        <TouchableOpacity style={styles.getStartedButton} onPress={handleGetStarted}>
          <Text style={styles.getStartedText}>Get Started</Text>
        </TouchableOpacity>
      </View>
    </ImageBackground>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentContainer: {
    backgroundColor: 'rgba(255, 255, 255, 0.8)',
    padding: 24,
    borderRadius: 8,
    alignItems: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 12,
  },
  subtitle: {
    fontSize: 16,
    textAlign: 'center',
    marginBottom: 24,
  },
  getStartedButton: {
    backgroundColor: '#4CAF50',
    paddingVertical: 12,
    paddingHorizontal: 24,
    borderRadius: 8,
  },
  getStartedText: {
    color: '#fff',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default LandingScreen;
```
#Profile Screen File
```
import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet, FlatList, TouchableOpacity, Image } from 'react-native';

const user = {
  name: 'Jayadeep Mallela',
  email: 'malleljp@mail.uc.edu',
};

const ProfileScreen = ({ navigation }) => {
  const [bookings, setBookings] = useState([
    {
      id: '1',
      experience: {
        title: 'Cooking Class with a Local Chef',
      },
      date: '2023-04-15',
      guests: 2,
    },
    {
      id: '2',
      experience: {
        title: 'Street Art Tour',
      },
      date: '2023-05-01',
      guests: 3,
    },
    {
      id: '3',
      experience: {
        title: 'Craft Beer Tasting',
      },
      date: '2023-06-20',
      guests: 1,
    },
  ]);

  const renderBookingItem = ({ item }) => (
    <TouchableOpacity
      style={styles.bookingItem}
      onPress={() => navigation.navigate('BookingDetails', { booking: item })}
    >
      <View style={styles.bookingInfo}>
        <Text style={styles.bookingTitle}>{item.experience.title}</Text>
        <Text style={styles.bookingDate}>{new Date(item.date).toLocaleDateString()}</Text>
        <Text style={styles.bookingGuests}>{item.guests} guests</Text>
      </View>
    </TouchableOpacity>
  );

  return (
    <View style={styles.container}>
      <View style={styles.profileContainer}>
        <Text style={styles.name}>{user.name}</Text>
        <Text style={styles.email}>{user.email}</Text>
      </View>

      <View style={styles.bookingsContainer}>
        <Text style={styles.sectionTitle}>My Bookings</Text>
        {bookings.length > 0 ? (
          <FlatList
            data={bookings}
            keyExtractor={(item) => item.id}
            renderItem={renderBookingItem}
            contentContainerStyle={styles.bookingList}
          />
        ) : (
          <Text style={styles.noBookingsText}>You have no bookings yet.</Text>
        )}
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f2f2f2',
  },
  profileContainer: {
    backgroundColor: '#fff',
    padding: 16,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    elevation: 2,
  },
  avatar: {
    width: 80,
    height: 80,
    borderRadius: 40,
    marginBottom: 8,
  },
  name: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 4,
  },
  email: {
    fontSize: 16,
    color: '#666',
  },
  bookingsContainer: {
    flex: 1,
    padding: 16,
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 16,
  },
  bookingList: {
    paddingBottom: 16,
  },
  bookingItem: {
    backgroundColor: '#fff',
    borderRadius: 8,
    marginBottom: 16,
    flexDirection: 'row',
    overflow: 'hidden',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    elevation: 2,
  },
  bookingImage: {
    width: 100,
    height: 100,
  },
  bookingInfo: {
    flex: 1,
    padding: 12,
  },
  bookingTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    marginBottom: 4,
  },
  bookingDate: {
    fontSize: 14,
    color: '#666',
    marginBottom: 4,
  },
  bookingGuests: {
    fontSize: 14,
    color: '#666',
  },
});

export default ProfileScreen;
```

Landing Page Screenshot
![Landing Screen Screenshot](/LandingScreen.jpg)

Home Page Screenshot
![Home Screen Screenshot](/HomeScreen.jpg)

Experience Page Screenshot
![Experience Details Screen Screenshot](/ExperienceDetailsScreen.jpg)

Booking Page Screenshot
![Booking Screen Screenshot](/BookingScreen.jpg)

Confirmation Page Screenshot
![Confirmation Screen Screenshot](/ConfirmationScreen.jpg)

Profile Page Screenshot
![Profile Screen Screenshot](/ProfileScreen.jpg)

Brief Demonstartion of the App
[Brief Demonstartion of the App](/Brief_Demonstartion_of_the_App.mp4)
