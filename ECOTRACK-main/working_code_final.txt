import React, { useState, useCallback } from 'react';
import { 
  View, 
  Text, 
  StyleSheet, 
  ScrollView, 
  TouchableOpacity, 
  TextInput,
  Dimensions,
  KeyboardAvoidingView,
  Platform,
  Image,
  Modal,
  ActivityIndicator,
  FlatList
} from 'react-native';
import { LinearGradient } from 'expo-linear-gradient';
import { MaterialCommunityIcons, Ionicons, AntDesign, FontAwesome5, MaterialIcons } from '@expo/vector-icons';

const WelcomeQuoteScreen = ({ onClose }) => (
  <View style={styles.quoteContainer}>
    <TouchableOpacity style={styles.closeButton} onPress={onClose}>
      <AntDesign name="close" size={24} color="#2E7D32" />
    </TouchableOpacity>
    <Text style={styles.quote}>
      "The greatest threat to our planet is the belief that someone else will save it."
    </Text>
    <Text style={styles.quoteAuthor}>- Robert Swan</Text>
  </View>
);

const DashboardTab = ({ onTipsPress, onCalculatorPress }) => (
  <ScrollView 
    style={styles.tabContent}
    showsVerticalScrollIndicator={true}
    contentContainerStyle={styles.scrollContentContainer}
  >
    {/* Carbon Impact Card */}
    <View style={styles.carbonCard}>
      <LinearGradient
        colors={['#2E7D32', '#4CAF50']}
        style={styles.carbonGradient}
      >
        <View style={styles.carbonHeader}>
          <Text style={styles.carbonTitle}>Your Carbon Impact</Text>
          <MaterialCommunityIcons name="information" size={24} color="white" />
        </View>
        <View style={styles.carbonContent}>
          <View style={styles.carbonCircle}>
            <Text style={styles.carbonValue}>2.5</Text>
            <Text style={styles.carbonUnit}>tons CO₂‚‚</Text>
          </View>
          <View style={styles.carbonStats}>
            <View style={styles.carbonStat}>
              <Text style={styles.carbonStatLabel}>Monthly Goal</Text>
              <Text style={styles.carbonStatValue}>3.0 tons</Text>
            </View>
            <View style={styles.carbonStat}>
              <Text style={styles.carbonStatLabel}>Reduction</Text>
              <Text style={styles.carbonStatValue}>-15%</Text>
            </View>
          </View>
        </View>
      </LinearGradient>
    </View>

    {/* Achievement Cards */}
    <View style={styles.achievementContainer}>
      <Text style={styles.sectionTitle}>Recent Achievements</Text>
      <ScrollView horizontal showsHorizontalScrollIndicator={false} style={styles.achievementScroll}>        {[
          { 
            icon: 'bike', 
            title: 'Cycle Champion', 
            description: 'Cycled 50km this month',
            date: '2 days ago'
          },
          { 
            icon: 'lightbulb', 
            title: 'Energy Saver', 
            description: 'Reduced energy by 20%',
            date: '1 week ago'
          },
          { 
            icon: 'recycle', 
            title: 'Waste Warrior', 
            description: 'Zero waste for a week',
            date: '2 weeks ago'
          },
          { 
            icon: 'solar-power', 
            title: 'Solar Pioneer', 
            description: 'Started using solar panels',
            date: '3 weeks ago'
          },
          { 
            icon: 'food-apple', 
            title: 'Local Food Hero', 
            description: 'Bought local produce only',
            date: '1 month ago'
          },
          { 
            icon: 'water', 
            title: 'Water Saver', 
            description: 'Reduced water usage by 30%',
            date: '1 month ago'
          },
          { 
            icon: 'tree', 
            title: 'Tree Planter', 
            description: 'Planted 5 trees this month',
            date: '1 month ago'
          },
          { 
            icon: 'car-electric', 
            title: 'EV Adopter', 
            description: 'Switched to electric vehicle',
            date: '2 months ago'
          }
        ].map((achievement, index) => (
          <View key={index} style={styles.achievementCard}>
            <View style={styles.achievementIcon}>
              <MaterialCommunityIcons name={achievement.icon} size={30} color="#2E7D32" />
            </View>
            <Text style={styles.achievementTitle}>{achievement.title}</Text>            <Text style={styles.achievementDesc}>{achievement.description}</Text>
            <Text style={styles.achievementDate}>{achievement.date}</Text>
          </View>
        ))}
      </ScrollView>
    </View>

    {/* Quick Actions */}
    <View style={styles.quickActions}>
      <Text style={styles.sectionTitle}>Quick Actions</Text>
      <View style={styles.actionGrid}>        {[          { 
            icon: 'calculator', 
            title: 'Calculate', 
            color: '#E8F5E9',
            onPress: () => onCalculatorPress()
          },
          { 
            icon: 'chart-line', 
            title: 'Track', 
            color: '#E8F5E9',
            onPress: () => {}
          },
          { 
            icon: 'lightbulb-on', 
            title: 'Tips', 
            color: '#E8F5E9',            onPress: onTipsPress
          },
          { 
            icon: 'gift', 
            title: 'Rewards', 
            color: '#E8F5E9',
            onPress: () => setActiveTab('rewards')
          }
        ].map((action, index) => (
          <TouchableOpacity 
            key={index} 
            style={[styles.actionCard, { backgroundColor: action.color }]}
            onPress={action.onPress}
          >
            <MaterialCommunityIcons name={action.icon} size={32} color="#2E7D32" />
            <Text style={styles.actionTitle}>{action.title}</Text>
          </TouchableOpacity>
        ))}
      </View>
    </View>

    {/* Daily Tips */}  <View style={styles.tipsContainer}>
      <Text style={styles.sectionTitle}>Today's Eco Tips</Text>
      <ScrollView 
        style={styles.tipsScrollView}
        showsVerticalScrollIndicator={true}
      >
        {[
          { 
            icon: 'car-electric',
            title: 'Transportation',
            tip: 'Consider carpooling today to reduce emissions',
            impact: '-2.5 kg COâ‚‚',
            points: 50,
            completed: false
          },
          {
            icon: 'food-apple',
            title: 'Diet',
            tip: 'Try a plant-based meal for lunch',
            impact: '-1.8 kg COâ‚‚',
            points: 30,
            completed: false
          },
          {
            icon: 'lightbulb-outline',
            title: 'Energy',
            tip: 'Switch off unused electronics and lights',
            impact: '-1.2 kg COâ‚‚',
            points: 20,
            completed: false
          },
          {
            icon: 'water',
            title: 'Water Usage',
            tip: 'Take a shorter shower (under 5 minutes)',
            impact: '-0.5 kg COâ‚‚',
            points: 25,
            completed: false
          },
          {
            icon: 'recycle',
            title: 'Waste',
            tip: 'Properly segregate waste for recycling',
            impact: '-1.0 kg COâ‚‚',
            points: 35,
            completed: false
          },
          {
            icon: 'shopping',
            title: 'Shopping',
            tip: 'Use reusable bags for grocery shopping',
            impact: '-0.8 kg COâ‚‚',
            points: 15,
            completed: false
          },
          {
            icon: 'home',
            title: 'Home',
            tip: 'Adjust thermostat by 2 degrees for efficiency',
            impact: '-1.5 kg COâ‚‚',
            points: 40,
            completed: false
          },
          {
            icon: 'bottle-wine',
            title: 'Lifestyle',
            tip: 'Use a reusable water bottle today',
            impact: '-0.3 kg COâ‚‚',
            points: 20,
            completed: false
          }
        ].map((tip, index) => (
          <TouchableOpacity 
            key={index} 
            style={[styles.tipCard, tip.completed && styles.completedTipCard]}            onPress={() => {              if (!tip.completed) {
                tip.completed = true;
                Alert.alert(
                  "Task Completed!",
                  `You earned ${tip.points} eco points!`,
                  [{ text: "OK", onPress: () => console.log("OK Pressed") }]
                );}
            }}
          >
            <View style={styles.tipHeader}>
              <View style={styles.tipIconContainer}>
                <MaterialCommunityIcons 
                  name={tip.completed ? "check-circle" : tip.icon} 
                  size={24} 
                  color={tip.completed ? "#4CAF50" : "#2E7D32"} 
                />
              </View>
              <View style={styles.tipContent}>
                <Text style={[styles.tipTitle, tip.completed && styles.completedText]}>
                  {tip.title}
                </Text>
                <Text style={[styles.tipText, tip.completed && styles.completedText]}>
                  {tip.tip}
                </Text>
              </View>
            </View>
            <View style={styles.tipImpact}>
              <View style={styles.impactContainer}>
                <Text style={styles.impactText}>{tip.impact}</Text>
                <Text style={styles.pointsText}>+{tip.points} points</Text>
              </View>
              <MaterialCommunityIcons 
                name={tip.completed ? "check-circle" : "chevron-right"} 
                size={24} 
                color={tip.completed ? "#4CAF50" : "#2E7D32"} 
              />
            </View>
          </TouchableOpacity>
        ))}
      </ScrollView>
    </View>  </ScrollView>
);

const AIRecommendationsTab = () => {
  const [ecoPoints, setEcoPoints] = useState(850);
  const [completedTips, setCompletedTips] = useState(new Set());
  
  const handleTipCompletion = (categoryIndex, tipIndex) => {    
    const tipId = `${categoryIndex}-${tipIndex}`;
    if (!completedTips.has(tipId)) {
      const newCompletedTips = new Set(completedTips);
      newCompletedTips.add(tipId);
      setCompletedTips(newCompletedTips);
      setEcoPoints(prevPoints => prevPoints + 5);
      
      Alert.alert(
        "Task Completed!",
        "You earned 5 eco points!",
        [{ text: "OK", onPress: () => console.log("OK Pressed") }]
      );
    }
  };

  const aiTips = [
    {
      title: 'Transportation',
      icon: 'car-electric',
      tips: [
        'Consider carpooling to work',
        'Use public transportation when possible',
        'Switch to an electric vehicle',
        'Start a bike-to-work initiative',
        'Organize a neighborhood carpool system'
      ]
    },
    {
      title: 'Home Energy',
      icon: 'home-lightning-bolt',
      tips: [
        'Install a smart thermostat',
        'Use natural light during day',
        'Unplug unused electronics',
        'Switch to LED bulbs',
        'Install solar panels',
        'Use energy-efficient appliances'
      ]
    },
    {
      title: 'Diet Choices',
      icon: 'food-apple',
      tips: [
        'Try meatless Mondays',
        'Buy local produce',
        'Reduce food waste',
        'Start composting',
        'Grow your own herbs',
        'Support local farmers markets'
      ]
    },
    {
      title: 'Water Conservation',
      icon: 'water',
      tips: [
        'Install water-efficient fixtures',
        'Collect rainwater for plants',
        'Fix leaky faucets',
        'Take shorter showers',
        'Use drought-resistant plants'
      ]
    },
    {
      title: 'Waste Management',
      icon: 'recycle',
      tips: [
        'Start a recycling program',
        'Use reusable shopping bags',
        'Avoid single-use plastics',
        'Create a compost bin',
        'Donate unused items'
      ]
    }
  ];

  return (
    <ScrollView style={styles.tabContent}>
      <View style={styles.pointsDisplay}>
        <MaterialCommunityIcons name="star-circle" size={24} color="#2E7D32" />
        <Text style={styles.pointsText}>Current Eco Points: {ecoPoints}</Text>
      </View>
      
      {aiTips.map((category, categoryIndex) => (
        <View key={categoryIndex} style={styles.recommendationCard}>
          <View style={styles.categoryHeader}>
            <MaterialCommunityIcons name={category.icon} size={32} color="#2E7D32" />
            <Text style={styles.recommendationTitle}>{category.title}</Text>
          </View>          {category.tips.map((tip, tipIndex) => {
            const tipId = `${categoryIndex}-${tipIndex}`;
            const isCompleted = completedTips.has(tipId);
            
            return (
              <TouchableOpacity
                key={tipIndex}
                style={[styles.tipItem, isCompleted && styles.completedTipItem]}
                onPress={() => handleTipCompletion(categoryIndex, tipIndex)}
                disabled={isCompleted}
              >
                <MaterialCommunityIcons
                  name={isCompleted ? "checkbox-marked" : "checkbox-blank-outline"}
                  size={24}
                  color={isCompleted ? "#4CAF50" : "#666"}
                />
                <Text style={[styles.tipItemText, isCompleted && styles.completedTipText]}>
                  {tip}
                </Text>
                {!isCompleted && (
                  <View style={styles.pointsBadge}>
                    <Text style={styles.pointsBadgeText}>+5</Text>
                  </View>
                )}
              </TouchableOpacity>
            );
          })}
        </View>
      ))}
    </ScrollView>
  );
};

const ProfileTab = () => {
  const [activeSection, setActiveSection] = useState('profile');
  const [showInviteModal, setShowInviteModal] = useState(false);
  const [selectedChallenge, setSelectedChallenge] = useState(null);
  const [joinedChallenges, setJoinedChallenges] = useState(new Set());
  const [showSettings, setShowSettings] = useState(false);
  const [isDarkMode, setIsDarkMode] = useState(false);

  // Dummy data for challenges  const [expandedChallenge, setExpandedChallenge] = useState(null);
  const challenges = [
    {
      id: '1',
      title: 'Plastic-Free Week',
      description: 'Avoid single-use plastics for 7 days. Track your progress and share tips with other participants!',
      duration: '7 days',
      participants: 1234,
      startDate: '2024-02-25',
      endDate: '2024-03-03',
      points: 500,
      progress: 70,
      type: 'ongoing',
      tasks: [
        'Use reusable shopping bags',
        'Carry a water bottle',
        'Say no to plastic straws',
        'Choose plastic-free packaging'
      ],
      leaderboard: [
        { name: 'Emma S.', points: 450, rank: 1, avatar: '👑' },
        { name: 'John D.', points: 420, rank: 2, avatar: '🥈' },
        { name: 'Sarah M.', points: 380, rank: 3, avatar: '🥉' },
        { name: 'Hariharan', points: 350, rank: 4, avatar: '🌟' },
        { name: 'Mike P.', points: 320, rank: 5, avatar: '⭐' }
      ]
    },
    {
      id: '2',
      title: 'Bike to Work Challenge',
      description: 'Replace car trips with bicycle for commuting. Good for the planet and your health!',
      duration: '30 days',
      participants: 856,
      startDate: '2024-03-01',
      endDate: '2024-03-30',
      points: 1000,
      progress: 30,
      type: 'ongoing',
      tasks: [
        'Bike minimum 3 days per week',
        'Log your mileage',
        'Share your route',
        'Post cycling photos'
      ],
      leaderboard: [
        { name: 'Mike R.', points: 800, rank: 1, avatar: '👑' },
        { name: 'Lisa K.', points: 750, rank: 2, avatar: '🥈' },
        { name: 'Hariharan', points: 700, rank: 3, avatar: '🥉' },
        { name: 'David P.', points: 650, rank: 4, avatar: '🌟' },
        { name: 'Anna S.', points: 600, rank: 5, avatar: '⭐' }
      ]
    },
    {
      id: '3',
      title: 'Zero Waste Month',
      description: 'Minimize household waste generation. Learn composting and recycling techniques!',
      duration: '30 days',
      participants: 2341,
      startDate: '2024-02-01',
      endDate: '2024-02-29',
      points: 1500,
      progress: 100,
      type: 'completed',
      tasks: [
        'Start composting',
        'Recycle properly',
        'Buy in bulk',
        'Use reusable containers'
      ],
      leaderboard: [
        { name: 'Alex T.', points: 1400, rank: 1, avatar: '👑' },
        { name: 'Maria G.', points: 1350, rank: 2, avatar: '🥈' },
        { name: 'James F.', points: 1300, rank: 3, avatar: '🥉' },
        { name: 'Hariharan', points: 1250, rank: 4, avatar: '🌟' },
        { name: 'Sophie R.', points: 1200, rank: 5, avatar: '⭐' }
      ]
    },
    {
      id: '4',
      title: 'Energy Saving Sprint',
      description: 'Reduce your household energy consumption through smart habits and efficient appliance use.',
      duration: '14 days',
      participants: 1567,
      startDate: '2024-03-10',
      endDate: '2024-03-24',
      points: 800,
      progress: 0,
      type: 'upcoming',
      tasks: [
        'Use LED bulbs',
        'Track energy usage',
        'Optimize thermostat',
        'Unplug idle devices'
      ],
      leaderboard: []
    },
    {
      id: '5',
      title: 'Local Food Challenge',
      description: 'Source your food from local producers and reduce your carbon footprint from food transportation.',
      duration: '21 days',
      participants: 943,
      startDate: '2024-03-15',
      endDate: '2024-04-05',
      points: 1200,
      progress: 0,
      type: 'upcoming',
      tasks: [
        'Visit farmers markets',
        'Cook seasonal meals',
        'Track food miles',
        'Share local recipes'
      ],
      leaderboard: []
    }
  ];

  const InviteModal = () => (
    <View style={styles.modalContainer}>
      <View style={styles.modalContent}>
        <View style={styles.modalHeader}>
          <Text style={styles.modalTitle}>Invite Friends</Text>
          <TouchableOpacity onPress={() => setShowInviteModal(false)}>
            <AntDesign name="close" size={24} color="#333" />
          </TouchableOpacity>
        </View>
        
        <Text style={styles.modalSubtitle}>
          Share this challenge with your friends
        </Text>

        <View style={styles.inviteOptions}>
          {[
            { icon: 'contacts', label: 'Contacts', color: '#4CAF50' },
            { icon: 'facebook-square', label: 'Facebook', color: '#1877F2' },
            { icon: 'twitter', label: 'Twitter', color: '#1DA1F2' },
            { icon: 'mail', label: 'Email', color: '#EA4335' }
          ].map((option, index) => (
            <TouchableOpacity 
              key={index} 
              style={styles.inviteOption}
              onPress={() => {
                toast.success(`Sharing via ${option.label}`);
                setShowInviteModal(false);
              }}
            >
              <AntDesign name={option.icon} size={32} color={option.color} />
              <Text style={styles.inviteOptionLabel}>{option.label}</Text>
            </TouchableOpacity>
          ))}
        </View>

        <View style={styles.referralSection}>
          <Text style={styles.referralTitle}>Your Referral Code</Text>
          <View style={styles.referralCode}>
            <Text style={styles.referralCodeText}>ECO-JOHN-2024</Text>
            <TouchableOpacity 
              onPress={() => {
                toast.success('Referral code copied!');
              }}
            >
              <MaterialCommunityIcons name="content-copy" size={24} color="#2E7D32" />
            </TouchableOpacity>
          </View>
        </View>
      </View>
    </View>
  );

  const renderProfile = () => (
    <View>
      <View style={styles.profileHeader}>        <View style={styles.profileImageContainer}>
          <MaterialCommunityIcons name="account-circle" size={100} color="#2E7D32" />
        </View>
        <Text style={styles.profileName}>Hariharan</Text>
        <Text style={styles.profileBadge}>Eco Warrior</Text>
      </View>

      <View style={styles.statsContainer}>
        <View style={styles.statItem}>
          <Text style={styles.statNumber}>850</Text>
          <Text style={styles.statLabel}>Eco Points</Text>
        </View>
        <View style={styles.statItem}>
          <Text style={styles.statNumber}>#42</Text>
          <Text style={styles.statLabel}>Ranking</Text>
        </View>
        <View style={styles.statItem}>
          <Text style={styles.statNumber}>15</Text>
          <Text style={styles.statLabel}>Achievements</Text>
        </View>
      </View>

      <View style={styles.leaderboardSection}>
        <Text style={styles.sectionTitle}>Leaderboard</Text>
        {[
          { name: 'Sarah M.', points: 1250, rank: 1 },
          { name: 'Mike R.', points: 1100, rank: 2 },
          { name: 'You', points: 850, rank: 42, isUser: true },
        ].map((user, index) => (
          <View key={index} style={[styles.leaderboardItem, user.isUser && styles.userLeaderboardItem]}>
            <Text style={styles.rankNumber}>#{user.rank}</Text>
            <Text style={styles.leaderboardName}>{user.name}</Text>
            <Text style={styles.leaderboardPoints}>{user.points} pts</Text>
          </View>
        ))}
      </View>
    </View>
  );

  const [expandedChallenge, setExpandedChallenge] = useState(null);

const renderChallenges = () => (
    <View style={styles.challengesContainer}>
      <View style={styles.challengesHeader}>
        <Text style={styles.challengesTitle}>Community Challenges</Text>
        <TouchableOpacity 
          style={styles.filterButton}
          onPress={() => {
            // Add filter functionality here
          }}
        >
          <MaterialCommunityIcons name="filter-variant" size={24} color="#2E7D32" />
        </TouchableOpacity>
      </View>

      {challenges.map((challenge, index) => (
        <View key={index} style={styles.challengeCard}>
          <View style={styles.challengeHeader}>
            <View style={styles.challengeTitleContainer}>
              <Text style={styles.challengeTitle}>{challenge.title}</Text>
              {challenge.type === 'completed' && (
                <MaterialCommunityIcons name="check-circle" size={24} color="#4CAF50" />
              )}
            </View>
            <Text style={styles.challengeDescription}>{challenge.description}</Text>
          </View>

          <View style={styles.challengeStats}>
            <View style={styles.challengeStat}>
              <MaterialCommunityIcons name="calendar-range" size={20} color="#666" />
              <Text style={styles.challengeStatText}>{challenge.duration}</Text>
            </View>
            <View style={styles.challengeStat}>
              <MaterialCommunityIcons name="account-group" size={20} color="#666" />
              <Text style={styles.challengeStatText}>{challenge.participants} joined</Text>
            </View>
            <View style={styles.challengeStat}>
              <MaterialCommunityIcons name="star" size={20} color="#666" />
              <Text style={styles.challengeStatText}>{challenge.points} points</Text>
            </View>
          </View>

          <View style={styles.progressContainer}>
            <View style={styles.progressBar}>
              <View 
                style={[
                  styles.progressFill, 
                  { width: `${challenge.progress}%` }
                ]} 
              />
            </View>
            <Text style={styles.progressText}>{challenge.progress}% Complete</Text>
          </View>

          <View style={styles.challengeActions}>
            {challenge.type === 'ongoing' ? (
              <>
                <TouchableOpacity 
                  style={[
                    styles.challengeButton,
                    joinedChallenges.has(challenge.id) && styles.challengeButtonJoined
                  ]}
                  onPress={() => {
                    if (!joinedChallenges.has(challenge.id)) {
                      const newJoinedChallenges = new Set(joinedChallenges);
                      newJoinedChallenges.add(challenge.id);
                      setJoinedChallenges(newJoinedChallenges);
                      toast.success('Successfully joined the challenge!');
                    }
                  }}
                >
                  <Text style={[
                    styles.challengeButtonText,
                    joinedChallenges.has(challenge.id) && styles.challengeButtonTextJoined
                  ]}>
                    {joinedChallenges.has(challenge.id) ? 'Joined' : 'Join Challenge'}
                  </Text>
                </TouchableOpacity>
                <TouchableOpacity 
                  style={[styles.inviteButton, !joinedChallenges.has(challenge.id) && styles.inviteButtonDisabled]}
                  onPress={() => {
                    if (joinedChallenges.has(challenge.id)) {
                      setSelectedChallenge(challenge);
                      setShowInviteModal(true);
                    }
                  }}
                  disabled={!joinedChallenges.has(challenge.id)}
                >
                  <MaterialCommunityIcons 
                    name="account-plus" 
                    size={24} 
                    color={joinedChallenges.has(challenge.id) ? "#2E7D32" : "#999"} 
                  />
                  <Text style={[
                    styles.inviteButtonText,
                    !joinedChallenges.has(challenge.id) && styles.inviteButtonTextDisabled
                  ]}>
                    Invite Friends
                  </Text>
                </TouchableOpacity>
              </>
            ) : (
              <View style={styles.completedChallenge}>
                <MaterialCommunityIcons name="trophy" size={24} color="#FFD700" />
                <Text style={styles.completedChallengeText}>Challenge Completed!</Text>
              </View>
            )}
          </View>          {/* Leaderboard Section */}
          <TouchableOpacity 
            style={styles.leaderboardToggle}
            onPress={() => setExpandedChallenge(expandedChallenge === challenge.id ? null : challenge.id)}
          >
            <Text style={styles.leaderboardToggleText}>
              {expandedChallenge === challenge.id ? 'Hide Leaderboard' : 'View Leaderboard'}
            </Text>
            <MaterialCommunityIcons 
              name={expandedChallenge === challenge.id ? "chevron-up" : "chevron-down"} 
              size={24} 
              color="#2E7D32" 
            />
          </TouchableOpacity>

          {/* Expanded Leaderboard */}
          {expandedChallenge === challenge.id && (
            <View style={styles.leaderboardContainer}>
              <Text style={styles.leaderboardTitle}>Top Participants</Text>
              {challenge.leaderboard.map((participant, index) => (
                <View 
                  key={index} 
                  style={[
                    styles.leaderboardRow,
                    participant.name === 'Hariharan' && styles.userLeaderboardRow
                  ]}
                >
                  <Text style={styles.leaderboardAvatar}>{participant.avatar}</Text>
                  <Text style={styles.leaderboardRank}>#{participant.rank}</Text>
                  <Text style={[
                    styles.leaderboardName,
                    participant.name === 'Hariharan' && styles.userLeaderboardText
                  ]}>
                    {participant.name}
                    {participant.name === 'Hariharan' && ' (You)'}
                  </Text>
                  <Text style={[
                    styles.leaderboardPoints,
                    participant.name === 'Hariharan' && styles.userLeaderboardText
                  ]}>
                    {participant.points} pts
                  </Text>
                </View>
              ))}
            </View>
          )}
        </View>
      ))}
    </View>
  );

  return (
    <View style={styles.container}>
      <View style={styles.tabBar}>
        <TouchableOpacity 
          style={[styles.tab, activeSection === 'profile' && styles.activeTab]}
          onPress={() => setActiveSection('profile')}
        >
          <MaterialCommunityIcons 
            name="account" 
            size={24} 
            color={activeSection === 'profile' ? '#2E7D32' : '#666'} 
          />
          <Text style={[styles.tabText, activeSection === 'profile' && styles.activeTabText]}>
            Profile
          </Text>
        </TouchableOpacity>
        <TouchableOpacity 
          style={[styles.tab, activeSection === 'challenges' && styles.activeTab]}
          onPress={() => setActiveSection('challenges')}
        >
          <MaterialCommunityIcons 
            name="trophy" 
            size={24} 
            color={activeSection === 'challenges' ? '#2E7D32' : '#666'} 
          />
          <Text style={[styles.tabText, activeSection === 'challenges' && styles.activeTabText]}>
            Challenges
          </Text>
        </TouchableOpacity>
      </View>

      <ScrollView style={styles.tabContent}>
        {activeSection === 'profile' ? renderProfile() : renderChallenges()}
      </ScrollView>      {showInviteModal && <InviteModal />}      {showSettings && <RenderSettingsModal />}
    </View>
  );
};

const RewardsTab = () => {
  const [availablePoints, setAvailablePoints] = useState(850);

  const rewards = [
    {
      title: 'Eco-friendly Water Bottle',
      points: 500,
      image: 'water-bottle',
      description: 'Reusable stainless steel bottle'
    },
    {
      title: 'Organic Market Voucher',
      points: 750,
      image: 'shopping-bag',
      description: '$25 voucher for organic products'
    },
    {
      title: 'Solar Power Bank',
      points: 1000,
      image: 'solar-power',
      description: 'Portable solar charging device'
    },
    {
      title: 'Eco-friendly Backpack',
      points: 1200,
      image: 'backpack',
      description: 'Made from recycled materials'
    },
    {
      title: 'Smart Plant Monitor',
      points: 800,
      image: 'flower',
      description: 'Track your plants health digitally'
    },
    {
      title: 'Bamboo Cutlery Set',
      points: 400,
      image: 'silverware',
      description: 'Sustainable dining essentials'
    },
    {
      title: 'Electric Bike Rental',
      points: 1500,
      image: 'bike-electric',
      description: '3-day e-bike rental pass'
    },
    {
      title: 'Tree Planting Kit',
      points: 600,
      image: 'tree',
      description: 'Plant your own mini forest'
    },
    {
      title: 'Solar LED Lights',
      points: 700,
      image: 'led-strip',
      description: 'Energy-efficient outdoor lighting'
    },
    {
      title: 'Composting Starter Kit',
      points: 550,
      image: 'compost',
      description: 'Begin your composting journey'
    },
    {
      title: 'Rain Water Collector',
      points: 900,
      image: 'water',
      description: 'Sustainable water collection system'
    },
    {
      title: 'Green Energy Certificate',
      points: 1300,
      image: 'certificate',
      description: 'Support renewable energy projects'
    },
    {
      title: 'Home Energy Monitor',
      points: 1100,
      image: 'home-lightning-bolt',
      description: 'Track your energy consumption'
    }
  ];  const handleClaim = (reward) => {
    if (availablePoints >= reward.points) {
      const newBalance = availablePoints - reward.points;
      setAvailablePoints(newBalance);
      Alert.alert(
        "Reward Claimed!",
        `You have successfully claimed ${reward.title}. Your new balance is ${newBalance} points.`,
        [
          {
            text: "OK",
            onPress: () => {
              console.log("Reward claimed");
              // You could add additional functionality here like updating a backend
            }
          }
        ]
      );
    } else {
      Alert.alert(
        "Insufficient Points",
        `You need ${reward.points - availablePoints} more points to claim this reward.`,
        [
          {
            text: "OK",
            onPress: () => console.log("Insufficient points alert closed")
          }
        ]
      );
    }
  };

  return (
    <ScrollView style={styles.tabContent}>
      <View style={styles.rewardsHeader}>
        <Text style={styles.rewardsTitle}>Available Rewards</Text>
        <Text style={styles.rewardsPoints}>{availablePoints} Points Available</Text>
      </View>

      {rewards.map((reward, index) => (
        <View key={index} style={styles.rewardCard}>
          <MaterialCommunityIcons name={reward.image} size={40} color="#2E7D32" />
          <View style={styles.rewardInfo}>
            <Text style={styles.rewardTitle}>{reward.title}</Text>
            <Text style={styles.rewardDescription}>{reward.description}</Text>
            <Text style={styles.rewardPoints}>{reward.points} points</Text>
          </View>
          <TouchableOpacity 
            style={[
              styles.claimButton,
              { opacity: reward.points <= availablePoints ? 1 : 0.5 }
            ]}
            disabled={reward.points > availablePoints}
            onPress={() => handleClaim(reward)}
          >
            <Text style={styles.claimButtonText}>Claim</Text>
          </TouchableOpacity>
        </View>
      ))}
    </ScrollView>
  );
};// AI Chatbot Component Definition
const FAQS = [
  {
    question: 'What is Eco Tracker?',
    answer: 'Eco Tracker is an app that helps users monitor and reduce their carbon footprint'
  },
  {
    question: 'What is a carbon footprint?',
    answer: 'A carbon footprint is the total amount of greenhouse gases produced by human activities, measured in CO2 equivalent'
  },
  {
    question: 'How can we reduce carbon usage using this app?',
    answer: 'You can track your activities, get AI recommendations, and complete eco-friendly challenges'
  },
  {
    question: 'What are the benefits of Eco Points?',
    answer: 'Eco Points reward users for sustainable actions, which can be redeemed for eco-friendly products'
  },
  {
    question: 'How do I complete tasks in the app?',
    answer: 'You can complete daily eco-tasks and challenges to earn Eco Points'
  }
];

const AIChatbot = () => {
  const [modalVisible, setModalVisible] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);
  const [showDropdown, setShowDropdown] = useState(false);  const [remainingQuestions, setRemainingQuestions] = useState([
    {
      question: 'What is Eco Tracker?',
      answer: 'Eco Tracker is an app that helps users monitor and reduce their carbon footprint'
    },
    {
      question: 'What is a carbon footprint?',
      answer: 'A carbon footprint is the total amount of greenhouse gases produced by human activities, measured in CO2 equivalent'
    },
    {
      question: 'How can we reduce carbon usage using this app?',
      answer: 'You can track your activities, get AI recommendations, and complete eco-friendly challenges'
    },
    {
      question: 'What are the benefits of Eco Points?',
      answer: 'Eco Points reward users for sustainable actions, which can be redeemed for eco-friendly products'
    },
    {
      question: 'How do I complete tasks in the app?',
      answer: 'You can complete daily eco-tasks and challenges to earn Eco Points'
    },
    {
      question: 'How does the Carbon Calculator work in Eco Tracker?',
      answer: 'The Carbon Calculator estimates your carbon footprint based on your transportation, energy use, food consumption, and waste habits. You can enter data manually, and the app will calculate your impact in CO₂ emissions'
    },
    {
      question: 'Can I switch between manual and automatic carbon tracking?',
      answer: 'Yes! Eco Tracker allows you to manually enter your activities or use automated tracking (if enabled) to estimate your emissions based on stored user data'
    },
    {
      question: 'How does the AI system in Eco Tracker provide recommendations?',
      answer: 'The AI analyzes your carbon footprint and provides personalized suggestions, such as reducing energy usage, switching to eco-friendly transport, and adopting sustainable habits'
    },
    {
      question: 'What happens if I don\'t complete my eco-tasks or challenges?',
      answer: 'There\'s no penalty for not completing tasks, but you won\'t earn Eco Points or progress in challenges. Completing tasks regularly helps you earn rewards and track your impact'
    },
    {
      question: 'Can I see my progress and carbon impact over time?',
      answer: 'Yes! The app provides a dashboard where you can view your weekly, monthly, and yearly carbon impact, helping you track improvements and set sustainability goals'
    }
  ]);

  const sendMessage = (text) => {
    if (!text.trim()) return;
    
    const userMessage = { text, sender: 'user' };
    setMessages((prev) => [...prev, userMessage]);
    setInput('');
    setLoading(true);

    setTimeout(() => {
      const faq = FAQS.find(faq => faq.question === text);
      const botReply = faq ? faq.answer : "I'm still learning! Try another question.";
      setMessages((prev) => [...prev, { text: botReply, sender: 'bot' }]);
      setLoading(false);
    }, 1000);
  };

  return (
    <View style={chatbotStyles.container}>
      <TouchableOpacity style={chatbotStyles.chatButton} onPress={() => setModalVisible(true)}>
        <MaterialCommunityIcons name="robot" size={28} color="white" />
      </TouchableOpacity>

      <Modal animationType="slide" transparent={true} visible={modalVisible}>
        <View style={chatbotStyles.modalContainer}>
          <View style={chatbotStyles.chatBox}>
            <View style={chatbotStyles.header}>
              <Text style={chatbotStyles.headerText}>AI Chatbot</Text>
              <TouchableOpacity onPress={() => setModalVisible(false)}>
                <MaterialCommunityIcons name="close" size={24} color="white" />
              </TouchableOpacity>
            </View>            <View style={chatbotStyles.chatContent}>
              {/* Dropdown Menu Button */}
              <TouchableOpacity 
                style={chatbotStyles.dropdownButton}
                onPress={() => setShowDropdown(!showDropdown)}
              >
                <Text style={chatbotStyles.dropdownButtonText}>
                  {showDropdown ? "Hide Questions" : "Show Available Questions"}
                </Text>
                <MaterialCommunityIcons 
                  name={showDropdown ? "chevron-up" : "chevron-down"} 
                  size={24} 
                  color="#2E7D32" 
                />
              </TouchableOpacity>

              {/* Dropdown Menu */}
              {showDropdown && (
                <View style={chatbotStyles.dropdownMenu}>
                  {remainingQuestions.map((faq, index) => (
                    <TouchableOpacity
                      key={index}
                      style={chatbotStyles.dropdownItem}
                      onPress={() => {
                        sendMessage(faq.question);
                        setShowDropdown(false);
                        setRemainingQuestions(remainingQuestions.filter(q => q.question !== faq.question));
                      }}
                    >
                      <MaterialCommunityIcons 
                        name="frequently-asked-questions" 
                        size={20} 
                        color="#2E7D32" 
                      />
                      <Text style={chatbotStyles.dropdownItemText}>{faq.question}</Text>
                    </TouchableOpacity>
                  ))}
                </View>
              )}

              {/* Messages */}
              <ScrollView style={chatbotStyles.messagesContainer}>
                <FlatList
                  data={messages}
                  renderItem={({ item }) => (
                    <View style={[
                      chatbotStyles.messageBubble, 
                      item.sender === 'user' ? chatbotStyles.userBubble : chatbotStyles.botBubble
                    ]}>
                      <Text style={chatbotStyles.messageText}>{item.text}</Text>
                    </View>
                  )}
                  keyExtractor={(item, index) => index.toString()}
                />
              </ScrollView>
            </View>

            {loading && <ActivityIndicator size="small" color="#4CAF50" style={chatbotStyles.typingIndicator} />}

            <View style={chatbotStyles.inputContainer}>
              <TextInput
                style={chatbotStyles.input}
                placeholder="Ask something..."
                value={input}
                onChangeText={setInput}
                onSubmitEditing={() => sendMessage(input)}
              />
              <TouchableOpacity style={chatbotStyles.sendButton} onPress={() => sendMessage(input)}>
                <MaterialCommunityIcons name="send" size={24} color="white" />
              </TouchableOpacity>
            </View>
          </View>
        </View>
      </Modal>
    </View>
  );
};

const chatbotStyles = StyleSheet.create({
  chatContent: {
    flex: 1,
  },
  dropdownButton: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'space-between',
    backgroundColor: '#E8F5E9',
    padding: 15,
    borderRadius: 10,
    marginBottom: 10,
    marginHorizontal: 15,
  },
  dropdownButtonText: {
    fontSize: 16,
    color: '#2E7D32',
    fontWeight: '600',
  },
  dropdownMenu: {
    backgroundColor: 'white',
    borderRadius: 10,
    marginHorizontal: 15,
    marginBottom: 10,
    padding: 5,
    maxHeight: 200,
    borderWidth: 1,
    borderColor: '#E0E0E0',
  },
  dropdownItem: {
    flexDirection: 'row',
    alignItems: 'center',
    padding: 12,
    borderRadius: 8,
    marginVertical: 2,
  },
  dropdownItemText: {
    marginLeft: 10,
    fontSize: 14,
    color: '#333',
    flex: 1,
  },
  suggestedQuestions: {
    padding: 15,
  },
  suggestedTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 10,
  },
  suggestedQuestion: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#E8F5E9',
    padding: 12,
    borderRadius: 10,
    marginBottom: 8,
  },
  suggestedQuestionText: {
    marginLeft: 10,
    fontSize: 14,
    color: '#2E7D32',
    flex: 1,
  },
  container: {
    position: 'absolute',
    top: 50,
    left: 20,
    zIndex: 100,
  },
  chatButton: {
    backgroundColor: '#2E7D32',
    padding: 12,
    borderRadius: 30,
    elevation: 5,
  },
  modalContainer: {
    flex: 1,
    backgroundColor: 'rgba(0,0,0,0.5)',
    justifyContent: 'center',
    alignItems: 'center',
  },
  chatBox: {
    width: '90%',
    backgroundColor: 'white',
    borderRadius: 20,
    overflow: 'hidden',
    elevation: 5,
  },
  header: {
    backgroundColor: '#2E7D32',
    padding: 15,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
  },
  headerText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  messagesContainer: {
    padding: 15,
    maxHeight: 400,
  },
  messageBubble: {
    padding: 12,
    borderRadius: 10,
    marginBottom: 10,
    maxWidth: '75%',
  },
  userBubble: {
    alignSelf: 'flex-end',
    backgroundColor: '#E8F5E9',
  },
  botBubble: {
    alignSelf: 'flex-start',
    backgroundColor: '#E3F2FD',
  },
  messageText: {
    fontSize: 16,
  },
  typingIndicator: {
    alignSelf: 'center',
    marginVertical: 5,
  },
  inputContainer: {
    flexDirection: 'row',
    padding: 10,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
  },
  input: {
    flex: 1,
    padding: 10,
    borderWidth: 1,
    borderColor: '#E0E0E0',
    borderRadius: 20,
    backgroundColor: 'white',
    marginRight: 10,
  },
  sendButton: {
    backgroundColor: '#2E7D32',
    padding: 10,
    borderRadius: 20,
  },
});

const EcoTipsModal = ({ visible, onClose }) => {
  const [selectedCategory, setSelectedCategory] = useState('all');
  
  const tips = {
    energy: [
      {
        title: 'Smart Temperature Control',
        description: 'Adjust your thermostat by just 1°C can reduce energy consumption by up to 10%',
        impact: '-2.5 kg CO₂/day',
        icon: 'thermometer'
      },
      {
        title: 'LED Lighting',
        description: 'Switch to LED bulbs to use 75% less energy than traditional bulbs',
        impact: '-1.8 kg CO₂/day',
        icon: 'lightbulb-on'
      },
      {
        title: 'Standby Power',
        description: 'Unplug electronics when not in use to avoid phantom energy consumption',
        impact: '-0.5 kg CO₂/day',
        icon: 'power-plug-off'
      }
    ],
    transportation: [
      {
        title: 'Eco-Driving',
        description: 'Maintain steady speed and avoid rapid acceleration to save fuel',
        impact: '-3.2 kg CO₂/day',
        icon: 'car-speed-limiter'
      },
      {
        title: 'Bike or Walk',
        description: 'Choose cycling or walking for short trips under 2km',
        impact: '-2.0 kg CO₂/trip',
        icon: 'bike'
      },
      {
        title: 'Public Transport',
        description: 'Using public transport can reduce your carbon footprint by up to 75%',
        impact: '-4.5 kg CO₂/day',
        icon: 'bus'
      }
    ],
    waste: [
      {
        title: 'Zero Waste Shopping',
        description: 'Bring reusable bags and containers for grocery shopping',
        impact: '-0.8 kg CO₂/day',
        icon: 'shopping-outline'
      },
      {
        title: 'Composting',
        description: 'Compost food scraps to reduce methane emissions from landfills',
        impact: '-1.2 kg CO₂/day',
        icon: 'leaf'
      },
      {
        title: 'Recycling',
        description: 'Properly segregate and recycle to save energy in production',
        impact: '-1.5 kg CO₂/day',
        icon: 'recycle'
      }
    ],
    food: [
      {
        title: 'Plant-Based Meals',
        description: 'Include more plant-based meals in your diet to reduce emissions',
        impact: '-4.0 kg CO₂/meal',
        icon: 'food-apple'
      },
      {
        title: 'Local Produce',
        description: 'Choose locally grown food to reduce transportation emissions',
        impact: '-1.0 kg CO₂/meal',
        icon: 'store'
      },
      {
        title: 'Food Waste',
        description: 'Plan meals and use leftovers to reduce food waste',
        impact: '-2.5 kg CO₂/day',
        icon: 'food'
      }
    ],
    water: [
      {
        title: 'Water Conservation',
        description: 'Fix leaks and use water-efficient appliances',
        impact: '-1.0 kg CO₂/day',
        icon: 'water'
      },
      {
        title: 'Short Showers',
        description: 'Take shorter showers to reduce hot water usage',
        impact: '-1.2 kg CO₂/day',
        icon: 'shower'
      },
      {
        title: 'Cold Wash',
        description: 'Use cold water for laundry when possible',
        impact: '-0.8 kg CO₂/load',
        icon: 'washing-machine'
      }
    ]
  };

  return (
    <Modal
      animationType="slide"
      transparent={true}
      visible={visible}
      onRequestClose={onClose}
    >
      <View style={styles.modalContainer}>
        <View style={styles.tipsModalContent}>
          <View style={styles.tipsHeader}>
            <Text style={styles.tipsTitle}>Eco-Friendly Tips</Text>
            <TouchableOpacity onPress={onClose}>
              <AntDesign name="close" size={24} color="#333" />
            </TouchableOpacity>
          </View>

          <ScrollView 
            horizontal 
            showsHorizontalScrollIndicator={false}
            style={styles.categoryScroll}
          >
            <TouchableOpacity 
              style={[
                styles.categoryButton,
                selectedCategory === 'all' && styles.selectedCategory
              ]}
              onPress={() => setSelectedCategory('all')}
            >
              <MaterialCommunityIcons 
                name="earth" 
                size={24} 
                color={selectedCategory === 'all' ? '#fff' : '#2E7D32'} 
              />
              <Text style={[
                styles.categoryText,
                selectedCategory === 'all' && styles.selectedCategoryText
              ]}>All</Text>
            </TouchableOpacity>
            {Object.keys(tips).map((category) => (
              <TouchableOpacity 
                key={category}
                style={[
                  styles.categoryButton,
                  selectedCategory === category && styles.selectedCategory
                ]}
                onPress={() => setSelectedCategory(category)}
              >
                <MaterialCommunityIcons 
                  name={
                    category === 'energy' ? 'lightning-bolt' :
                    category === 'transportation' ? 'car' :
                    category === 'waste' ? 'delete' :
                    category === 'food' ? 'food' : 'water'
                  }
                  size={24}
                  color={selectedCategory === category ? '#fff' : '#2E7D32'}
                />
                <Text style={[
                  styles.categoryText,
                  selectedCategory === category && styles.selectedCategoryText
                ]}>{category.charAt(0).toUpperCase() + category.slice(1)}</Text>
              </TouchableOpacity>
            ))}
          </ScrollView>

          <ScrollView style={styles.tipsContent}>
            {(selectedCategory === 'all' 
              ? Object.values(tips).flat() 
              : tips[selectedCategory]
            ).map((tip, index) => (
              <View key={index} style={styles.tipCard}>
                <View style={styles.tipIconContainer}>
                  <MaterialCommunityIcons 
                    name={tip.icon} 
                    size={32} 
                    color="#2E7D32" 
                  />
                </View>
                <View style={styles.tipTextContainer}>
                  <Text style={styles.tipTitle}>{tip.title}</Text>
                  <Text style={styles.tipDescription}>{tip.description}</Text>
                  <View style={styles.impactContainer}>
                    <MaterialCommunityIcons 
                      name="earth" 
                      size={16} 
                      color="#2E7D32" 
                    />
                    <Text style={styles.impactText}>{tip.impact}</Text>
                  </View>
                </View>
              </View>
            ))}
          </ScrollView>
        </View>
      </View>
    </Modal>
  );
};

const CalculatorScreen = ({ onClose }) => {
  const [distance, setDistance] = useState('');
  const [vehicle, setVehicle] = useState('');
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState('');

  // Emission factors in kg CO2 per kilometer
  const EMISSION_FACTORS = {
    car: 0.192, // Average car emission
    bus: 0.089, // Public bus
    train: 0.041, // Electric train
    motorcycle: 0.103, // Average motorcycle
    bicycle: 0, // Zero emission
    walking: 0 // Zero emission
  };

  const vehicles = [
    { id: 'car', name: 'Car', icon: 'car', description: 'Standard gasoline car' },
    { id: 'bus', name: 'Bus', icon: 'bus', description: 'Public transportation' },
    { id: 'train', name: 'Train', icon: 'train', description: 'Electric train' },
    { id: 'motorcycle', name: 'Motorcycle', icon: 'motorcycle', description: 'Standard motorcycle' },
    { id: 'bicycle', name: 'Bicycle', icon: 'bike', description: 'Zero emission' },
    { id: 'walking', name: 'Walking', icon: 'walk', description: 'Zero emission' }
  ];

  const validateInput = () => {
    if (!distance) {
      Alert.alert('Error', 'Please enter a distance');
      return false;
    }
    if (!vehicle) {
      Alert.alert('Error', 'Please select a vehicle type');
      return false;
    }
    const distanceNum = parseFloat(distance);
    if (isNaN(distanceNum) || distanceNum <= 0) {
      Alert.alert('Error', 'Please enter a valid distance');
      return false;
    }
    return true;
  };  const calculateEmissions = () => {
    const getComparisonText = (emissions) => {
      if (emissions === 0) {
        return "You've made an eco-friendly choice with zero emissions! 🌱";
      } else if (emissions < 1) {
        return `This is equivalent to charging ${Math.round(emissions * 120)} smartphones! 📱`;
      } else if (emissions < 5) {
        return `This is like running a TV for ${Math.round(emissions * 10)} hours! 📺`;
      } else {
        return `This equals ${Math.round(emissions / 2)} trees needed to offset for a month! 🌳`;
      }
    };

    if (!validateInput()) {
      return;
    }

    setLoading(true);
    
    // Simulate API call with setTimeout
    setTimeout(() => {
      try {
        const distanceNum = parseFloat(distance);
        const emissions = distanceNum * EMISSION_FACTORS[vehicle];
        const carEmissions = distanceNum * EMISSION_FACTORS.car;
        const savings = vehicle !== 'car' ? carEmissions - emissions : 0;

        // Calculate additional environmental impacts
        const treesNeeded = Math.round(emissions * 0.5); // Rough estimate: 1 tree offsets 2kg CO2 per month
        const litersFuel = Math.round(emissions * 0.4); // Rough estimate: 1L fuel produces 2.5kg CO2
        const energyEquivalent = Math.round(emissions * 10); // kWh equivalent

        setResult({
          distance: distanceNum,
          emissions: emissions,
          vehicle: vehicle,
          savings: savings,
          comparisonText: getComparisonText(emissions),
          environmentalImpact: {
            treesNeeded,
            litersFuel,
            energyEquivalent
          }
        });

        // Show success message with detailed breakdown
        Alert.alert(
          'Calculation Complete',
          `Your journey will emit ${emissions.toFixed(2)} kg of CO₂\n\n` +
          `This is equivalent to:\n` +
          `• ${treesNeeded} trees needed for offset\n` +
          `• ${litersFuel}L of fuel consumed\n` +
          `• ${energyEquivalent} kWh of energy`,
          [{ text: 'View Details' }]
        );
      } catch (err) {
        Alert.alert('Error', 'An error occurred while calculating. Please try again.');
      } finally {
        setLoading(false);
      }
    }, 1000); // 1 second delay to show loading state
  };

  const getComparisonText = (emissions) => {
    if (emissions === 0) {
      return "You've made an eco-friendly choice with zero emissions! 🌱";
    } else if (emissions < 1) {
      return `This is equivalent to charging ${Math.round(emissions * 120)} smartphones! 📱`;
    } else if (emissions < 5) {
      return `This is like running a TV for ${Math.round(emissions * 10)} hours! 📺`;
    } else {
      return `This equals ${Math.round(emissions / 2)} trees needed to offset for a month! 🌳`;
    }
  };

  return (
    <View style={styles.calculatorContainer}>
      <View style={styles.calculatorHeader}>
        <TouchableOpacity style={styles.closeButton} onPress={onClose}>
          <AntDesign name="close" size={24} color="#2E7D32" />
        </TouchableOpacity>
        <Text style={styles.calculatorTitle}>Carbon Footprint Calculator</Text>
      </View>

      <ScrollView style={styles.calculatorContent}>
        <View style={styles.inputSection}>
          <Text style={styles.inputLabel}>Enter Distance (km)</Text>
          <TextInput
            style={styles.calculatorInput}
            placeholder="Enter distance"
            keyboardType="numeric"
            value={distance}
            onChangeText={setDistance}
          />
        </View>

        <View style={styles.vehicleSection}>
          <Text style={styles.inputLabel}>Select Vehicle Type</Text>
          <View style={styles.vehicleGrid}>
            {vehicles.map((v) => (
              <TouchableOpacity
                key={v.id}
                style={[
                  styles.vehicleOption,
                  vehicle === v.id && styles.vehicleOptionActive
                ]}
                onPress={() => setVehicle(v.id)}
              >
                <MaterialCommunityIcons
                  name={v.icon}
                  size={32}
                  color={vehicle === v.id ? '#fff' : '#2E7D32'}
                />
                <Text style={[
                  styles.vehicleOptionText,
                  vehicle === v.id && styles.vehicleOptionTextActive
                ]}>
                  {v.name}
                </Text>
              </TouchableOpacity>
            ))}
          </View>
        </View>

        {/* Calculate Button */}
        <TouchableOpacity
          style={[
            styles.calculateButton,
            (!distance || !vehicle) && styles.calculateButtonDisabled
          ]}
          onPress={calculateEmissions}
          disabled={!distance || !vehicle || loading}
        >
          {loading ? (
            <ActivityIndicator color="white" />
          ) : (
            <View style={styles.calculateButtonContent}>
              <MaterialCommunityIcons name="calculator" size={24} color="white" />
              <Text style={styles.calculateButtonText}>Calculate Emissions</Text>
            </View>
          )}
        </TouchableOpacity>

        {result && (
          <View style={styles.resultCard}>
            <View style={styles.resultHeader}>
              <MaterialCommunityIcons name="chart-line" size={24} color="#2E7D32" />
              <Text style={styles.resultHeaderText}>Calculation Results</Text>
            </View>

            <View style={styles.resultItem}>
              <MaterialCommunityIcons name="map-marker-distance" size={24} color="#2E7D32" />
              <View style={styles.resultItemContent}>
                <Text style={styles.resultLabel}>Total Distance</Text>
                <Text style={styles.resultValue}>{result.distance.toFixed(1)} km</Text>
              </View>
            </View>

            <View style={styles.resultItem}>
              <MaterialCommunityIcons name="molecule-co2" size={24} color="#2E7D32" />
              <View style={styles.resultItemContent}>
                <Text style={styles.resultLabel}>Carbon Emissions</Text>
                <Text style={styles.resultValue}>{result.emissions.toFixed(2)} kg CO₂</Text>
              </View>
            </View>

            {result.vehicle !== 'car' && result.savings > 0 && (
              <View style={styles.savingsCard}>
                <MaterialCommunityIcons name="leaf" size={24} color="#fff" />
                <Text style={styles.savingsText}>
                  You saved {result.savings.toFixed(2)} kg CO₂ compared to driving a car!
                </Text>
              </View>
            )}

            <View style={styles.comparisonCard}>
              <MaterialCommunityIcons name="information" size={24} color="#2E7D32" />
              <Text style={styles.comparisonText}>{result.comparisonText}</Text>
            </View>
          </View>
        )}
      </ScrollView>
    </View>
  );
};

const MainScreen = ({ onLogout }) => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [showCalculator, setShowCalculator] = useState(false);
  const [showSettings, setShowSettings] = useState(false);
  const [isDarkMode, setIsDarkMode] = useState(false);
  const [showTips, setShowTips] = useState(false);
  const [carbonImpact, setCarbonImpact] = useState({
    current: 2.5,
    monthly: 3.0,
    reduction: -15
  });

  const handleTipsPress = () => {
    setShowTips(true);
  };

  const handleLogout = useCallback(() => {
    setShowSettings(false);
    if (onLogout) {
      onLogout();
    }
  }, [onLogout]);  const RenderSettingsModal = () => (
    <View style={styles.settingsModalContainer}>
      <View style={[styles.settingsModalContent, isDarkMode && styles.darkModeBackground]}>
        <View style={styles.settingsHeader}>
          <Text style={[styles.settingsTitle, isDarkMode && styles.darkModeText]}>Settings</Text>
          <TouchableOpacity onPress={() => setShowSettings(false)}>
            <AntDesign name="close" size={24} color={isDarkMode ? "#fff" : "#333"} />
          </TouchableOpacity>
        </View>

        <View style={styles.settingsOptions}>
          <TouchableOpacity 
            style={[styles.settingsOption, isDarkMode && styles.darkModeOption]}
            onPress={() => setIsDarkMode(false)}
          >
            <Ionicons 
              name="sunny" 
              size={24} 
              color={isDarkMode ? "#fff" : "#333"} 
              style={styles.settingsIcon}
            />
            <Text style={[styles.settingsOptionText, isDarkMode && styles.darkModeText]}>Light Mode</Text>
            {!isDarkMode && <AntDesign name="check" size={20} color="#2E7D32" />}
          </TouchableOpacity>

          <TouchableOpacity 
            style={[styles.settingsOption, isDarkMode && styles.darkModeOption]}
            onPress={() => setIsDarkMode(true)}
          >
            <Ionicons 
              name="moon" 
              size={24} 
              color={isDarkMode ? "#fff" : "#333"} 
              style={styles.settingsIcon}
            />
            <Text style={[styles.settingsOptionText, isDarkMode && styles.darkModeText]}>Dark Mode</Text>
            {isDarkMode && <AntDesign name="check" size={20} color="#4CAF50" />}
          </TouchableOpacity>

          <TouchableOpacity 
            style={[styles.logoutButton, isDarkMode && styles.darkModeLogoutButton]}            onPress={handleLogout}
          >
            <MaterialCommunityIcons 
              name="logout" 
              size={24} 
              color="#fff" 
              style={styles.settingsIcon}
            />
            <Text style={styles.logoutButtonText}>Logout</Text>
          </TouchableOpacity>
        </View>
      </View>
    </View>
  );
  
  const renderTabContent = () => {
    switch(activeTab) {      case 'dashboard':        
        return <DashboardTab 
          onTipsPress={() => setShowTips(true)} 
          onCalculatorPress={() => setShowCalculator(true)}
        />;
      case 'ai':
        return <AIRecommendationsTab />;
      case 'profile':
        return <ProfileTab />;
      case 'rewards':
        return <RewardsTab />;
      default:        return <DashboardTab onTipsPress={() => setShowTips(true)} />;
    }
  };  return (
    <View style={[styles.mainContainer, isDarkMode && styles.darkModeContainer]}>
      <AIChatbot />
      <View style={[styles.header, isDarkMode && styles.darkModeHeader]}>
        <View style={styles.headerRight}>
          <TouchableOpacity 
            style={styles.settingsButton}
            onPress={() => setShowSettings(true)}
          >
            <Ionicons 
              name="settings-sharp" 
              size={24} 
              color={isDarkMode ? "#fff" : "#333"} 
            />
          </TouchableOpacity>
        </View>
      </View>      {showSettings && <RenderSettingsModal />}
      {showCalculator ? (
        <CalculatorScreen onClose={() => setShowCalculator(false)} />
      ) : (
        renderTabContent()
      )}
      <EcoTipsModal 
        visible={showTips} 
        onClose={() => setShowTips(false)} 
      />
      <View style={styles.tabBar}>
        <TouchableOpacity 
          style={[styles.tab, activeTab === 'dashboard' && styles.activeTab]} 
          onPress={() => setActiveTab('dashboard')}
        >
          <MaterialCommunityIcons 
            name="view-dashboard" 
            size={24} 
            color={activeTab === 'dashboard' ? '#2E7D32' : '#666'} 
          />
          <Text style={[styles.tabText, activeTab === 'dashboard' && styles.activeTabText]}>
            Dashboard
          </Text>
        </TouchableOpacity>
        
        <TouchableOpacity 
          style={[styles.tab, activeTab === 'ai' && styles.activeTab]} 
          onPress={() => setActiveTab('ai')}
        >
          <MaterialCommunityIcons 
            name="robot" 
            size={24} 
            color={activeTab === 'ai' ? '#2E7D32' : '#666'} 
          />
          <Text style={[styles.tabText, activeTab === 'ai' && styles.activeTabText]}>
            AI Tips
          </Text>
        </TouchableOpacity>

        <TouchableOpacity 
          style={[styles.tab, activeTab === 'profile' && styles.activeTab]} 
          onPress={() => setActiveTab('profile')}
        >
          <MaterialCommunityIcons 
            name="account" 
            size={24} 
            color={activeTab === 'profile' ? '#2E7D32' : '#666'} 
          />
          <Text style={[styles.tabText, activeTab === 'profile' && styles.activeTabText]}>
            Profile
          </Text>
        </TouchableOpacity>

        <TouchableOpacity 
          style={[styles.tab, activeTab === 'rewards' && styles.activeTab]} 
          onPress={() => setActiveTab('rewards')}
        >
          <MaterialCommunityIcons 
            name="gift" 
            size={24} 
            color={activeTab === 'rewards' ? '#2E7D32' : '#666'} 
          />
          <Text style={[styles.tabText, activeTab === 'rewards' && styles.activeTabText]}>
            Rewards
          </Text>
        </TouchableOpacity>
      </View>
    </View>
  );
};

export default function App() {
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  const [showChat, setShowChat] = useState(false);
  
  const handleLogout = () => {
    setIsAuthenticated(false);
  };
  const [showWelcome, setShowWelcome] = useState(false);
  const [isSignIn, setIsSignIn] = useState(true);
  const [showCountries, setShowCountries] = useState(false);
  const [formData, setFormData] = useState({
    email: '',
    password: '',
    username: '',
    country: '',
    mobile: ''
  });
  const [errors, setErrors] = useState({});

  const validateForm = () => {
    let newErrors = {};
    
    if (!formData.email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }

    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }

    if (!isSignIn) {
      if (!formData.username) {
        newErrors.username = 'Username is required';
      }
      if (!formData.country) {
        newErrors.country = 'Country is required';
      }
      if (!formData.mobile) {
        newErrors.mobile = 'Mobile number is required';
      } else if (!/^\d{10}$/.test(formData.mobile)) {
        newErrors.mobile = 'Invalid mobile number';
      }
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  // Carbon calculation constants
const CalculatorScreen = ({ onClose }) => {
  const [distance, setDistance] = useState('');
  const [vehicle, setVehicle] = useState('');
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState('');

  // Emission factors in kg CO2 per kilometer
  const EMISSION_FACTORS = {
    car: 0.192, // Average car emission
    bus: 0.089, // Public bus
    train: 0.041, // Electric train
    motorcycle: 0.103, // Average motorcycle
    bicycle: 0, // Zero emission
    walking: 0 // Zero emission
  };

  const vehicles = [
    { id: 'car', name: 'Car', icon: 'car', description: 'Standard gasoline car' },
    { id: 'bus', name: 'Bus', icon: 'bus', description: 'Public transportation' },
    { id: 'train', name: 'Train', icon: 'train', description: 'Electric train' },
    { id: 'motorcycle', name: 'Motorcycle', icon: 'motorcycle', description: 'Standard motorcycle' },
    { id: 'bicycle', name: 'Bicycle', icon: 'bike', description: 'Zero emission' },
    { id: 'walking', name: 'Walking', icon: 'walk', description: 'Zero emission' }
  ];

  const validateInput = () => {
    if (!distance) {
      Alert.alert('Error', 'Please enter a distance');
      return false;
    }
    if (!vehicle) {
      Alert.alert('Error', 'Please select a vehicle type');
      return false;
    }
    const distanceNum = parseFloat(distance);
    if (isNaN(distanceNum) || distanceNum <= 0) {
      Alert.alert('Error', 'Please enter a valid distance');
      return false;
    }
    return true;
  };  const calculateEmissions = () => {
    const getComparisonText = (emissions) => {
      if (emissions === 0) {
        return "You've made an eco-friendly choice with zero emissions! 🌱";
      } else if (emissions < 1) {
        return `This is equivalent to charging ${Math.round(emissions * 120)} smartphones! 📱`;
      } else if (emissions < 5) {
        return `This is like running a TV for ${Math.round(emissions * 10)} hours! 📺`;
      } else {
        return `This equals ${Math.round(emissions / 2)} trees needed to offset for a month! 🌳`;
      }
    };

    if (!validateInput()) {
      return;
    }

    setLoading(true);
    
    // Simulate API call with setTimeout
    setTimeout(() => {
      try {
        const distanceNum = parseFloat(distance);
        const emissions = distanceNum * EMISSION_FACTORS[vehicle];
        const carEmissions = distanceNum * EMISSION_FACTORS.car;
        const savings = vehicle !== 'car' ? carEmissions - emissions : 0;

        // Calculate additional environmental impacts
        const treesNeeded = Math.round(emissions * 0.5); // Rough estimate: 1 tree offsets 2kg CO2 per month
        const litersFuel = Math.round(emissions * 0.4); // Rough estimate: 1L fuel produces 2.5kg CO2
        const energyEquivalent = Math.round(emissions * 10); // kWh equivalent

        setResult({
          distance: distanceNum,
          emissions: emissions,
          vehicle: vehicle,
          savings: savings,
          comparisonText: getComparisonText(emissions),
          environmentalImpact: {
            treesNeeded,
            litersFuel,
            energyEquivalent
          }
        });

        // Show success message with detailed breakdown
        Alert.alert(
          'Calculation Complete',
          `Your journey will emit ${emissions.toFixed(2)} kg of CO₂\n\n` +
          `This is equivalent to:\n` +
          `• ${treesNeeded} trees needed for offset\n` +
          `• ${litersFuel}L of fuel consumed\n` +
          `• ${energyEquivalent} kWh of energy`,
          [{ text: 'View Details' }]
        );
      } catch (err) {
        Alert.alert('Error', 'An error occurred while calculating. Please try again.');
      } finally {
        setLoading(false);
      }
    }, 1000); // 1 second delay to show loading state
  };

  const getComparisonText = (emissions) => {
    if (emissions === 0) {
      return "You've made an eco-friendly choice with zero emissions! 🌱";
    } else if (emissions < 1) {
      return `This is equivalent to charging ${Math.round(emissions * 120)} smartphones! 📱`;
    } else if (emissions < 5) {
      return `This is like running a TV for ${Math.round(emissions * 10)} hours! 📺`;
    } else {
      return `This equals ${Math.round(emissions / 2)} trees needed to offset for a month! 🌳`;
    }
  };

  return (
    <View style={styles.calculatorContainer}>
      <View style={styles.calculatorHeader}>
        <TouchableOpacity style={styles.closeButton} onPress={onClose}>
          <AntDesign name="close" size={24} color="#2E7D32" />
        </TouchableOpacity>
        <Text style={styles.calculatorTitle}>Carbon Footprint Calculator</Text>
      </View>

      <ScrollView style={styles.calculatorContent}>
        <View style={styles.inputSection}>
          <Text style={styles.inputLabel}>Enter Distance (km)</Text>
          <TextInput
            style={styles.calculatorInput}
            placeholder="Enter distance"
            keyboardType="numeric"
            value={distance}
            onChangeText={setDistance}
          />
        </View>

        <View style={styles.vehicleSection}>
          <Text style={styles.inputLabel}>Select Vehicle Type</Text>
          <View style={styles.vehicleGrid}>
            {vehicles.map((v) => (
              <TouchableOpacity
                key={v.id}
                style={[
                  styles.vehicleOption,
                  vehicle === v.id && styles.vehicleOptionActive
                ]}
                onPress={() => setVehicle(v.id)}
              >
                <MaterialCommunityIcons
                  name={v.icon}
                  size={32}
                  color={vehicle === v.id ? '#fff' : '#2E7D32'}
                />
                <Text style={[
                  styles.vehicleOptionText,
                  vehicle === v.id && styles.vehicleOptionTextActive
                ]}>
                  {v.name}
                </Text>
              </TouchableOpacity>
            ))}
          </View>
        </View>

        {/* Calculate Button */}
        <TouchableOpacity
          style={[
            styles.calculateButton,
            (!distance || !vehicle) && styles.calculateButtonDisabled
          ]}
          onPress={calculateEmissions}
          disabled={!distance || !vehicle || loading}
        >
          {loading ? (
            <ActivityIndicator color="white" />
          ) : (
            <View style={styles.calculateButtonContent}>
              <MaterialCommunityIcons name="calculator" size={24} color="white" />
              <Text style={styles.calculateButtonText}>Calculate Emissions</Text>
            </View>
          )}
        </TouchableOpacity>

        {result && (
          <View style={styles.resultCard}>
            <View style={styles.resultHeader}>
              <MaterialCommunityIcons name="chart-line" size={24} color="#2E7D32" />
              <Text style={styles.resultHeaderText}>Calculation Results</Text>
            </View>

            <View style={styles.resultItem}>
              <MaterialCommunityIcons name="map-marker-distance" size={24} color="#2E7D32" />
              <View style={styles.resultItemContent}>
                <Text style={styles.resultLabel}>Total Distance</Text>
                <Text style={styles.resultValue}>{result.distance.toFixed(1)} km</Text>
              </View>
            </View>

            <View style={styles.resultItem}>
              <MaterialCommunityIcons name="molecule-co2" size={24} color="#2E7D32" />
              <View style={styles.resultItemContent}>
                <Text style={styles.resultLabel}>Carbon Emissions</Text>
                <Text style={styles.resultValue}>{result.emissions.toFixed(2)} kg CO₂</Text>
              </View>
            </View>

            {result.vehicle !== 'car' && result.savings > 0 && (
              <View style={styles.savingsCard}>
                <MaterialCommunityIcons name="leaf" size={24} color="#fff" />
                <Text style={styles.savingsText}>
                  You saved {result.savings.toFixed(2)} kg CO₂ compared to driving a car!
                </Text>
              </View>
            )}

            <View style={styles.comparisonCard}>
              <MaterialCommunityIcons name="information" size={24} color="#2E7D32" />
              <Text style={styles.comparisonText}>{result.comparisonText}</Text>
            </View>
          </View>
        )}
      </ScrollView>
    </View>
  );
};

const handleSubmit = () => {
    if (validateForm()) {
      if (!isSignIn) {
        setShowWelcome(true);
      }
      setIsAuthenticated(true);
    }
  };

  if (isAuthenticated) {
    if (showWelcome) {
      return <WelcomeQuoteScreen onClose={() => setShowWelcome(false)} />;
    }    return <MainScreen onLogout={handleLogout} />;
  }

  return (
    <KeyboardAvoidingView 
      behavior={Platform.OS === "ios" ? "padding" : "height"}
      style={styles.container}
    >
      <ScrollView style={styles.scrollView}>
        <LinearGradient
          colors={['#2E7D32', '#4CAF50']}
          style={styles.header}
        >
          <Ionicons name="leaf" size={50} color="white" />
          <Text style={styles.headerTitle}>Eco Tracker</Text>
          <Text style={styles.headerSubtitle}>Track your carbon footprint</Text>
        </LinearGradient>

        <View style={styles.formContainer}>
          <View style={styles.tabContainer}>
            <TouchableOpacity 
              style={[styles.tab, isSignIn && styles.activeTab]}
              onPress={() => setIsSignIn(true)}
            >
              <Text style={[styles.tabText, isSignIn && styles.activeTabText]}>Sign In</Text>
            </TouchableOpacity>
            <TouchableOpacity 
              style={[styles.tab, !isSignIn && styles.activeTab]}
              onPress={() => setIsSignIn(false)}
            >
              <Text style={[styles.tabText, !isSignIn && styles.activeTabText]}>Sign Up</Text>
            </TouchableOpacity>
          </View>

          <View style={styles.inputContainer}>
            {!isSignIn && (
              <View style={styles.inputWrapper}>
                <MaterialCommunityIcons name="account" size={24} color="#666" style={styles.inputIcon} />
                <TextInput
                  style={styles.input}
                  placeholder="Username"
                  value={formData.username}
                  onChangeText={(text) => setFormData({...formData, username: text})}
                />
                {errors.username && <Text style={styles.errorText}>{errors.username}</Text>}
              </View>
            )}

            <View style={styles.inputWrapper}>
              <MaterialCommunityIcons name="email" size={24} color="#666" style={styles.inputIcon} />
              <TextInput
                style={styles.input}
                placeholder="Email"
                keyboardType="email-address"
                autoCapitalize="none"
                value={formData.email}
                onChangeText={(text) => setFormData({...formData, email: text})}
              />
              {errors.email && <Text style={styles.errorText}>{errors.email}</Text>}
            </View>

            <View style={styles.inputWrapper}>
              <MaterialCommunityIcons name="lock" size={24} color="#666" style={styles.inputIcon} />
              <TextInput
                style={styles.input}
                placeholder="Password"
                secureTextEntry
                value={formData.password}
                onChangeText={(text) => setFormData({...formData, password: text})}
              />
              {errors.password && <Text style={styles.errorText}>{errors.password}</Text>}
            </View>

            {!isSignIn && (
              <>
                <TouchableOpacity 
                  style={styles.inputWrapper}
                  onPress={() => setShowCountries(!showCountries)}
                >
                  <MaterialCommunityIcons name="earth" size={24} color="#666" style={styles.inputIcon} />
                  <Text style={[styles.input, !formData.country && styles.placeholder]}>
                    {formData.country || "Select Country"}
                  </Text>
                  {errors.country && <Text style={styles.errorText}>{errors.country}</Text>}
                </TouchableOpacity>

                {showCountries && (
                  <View style={styles.countryList}>
                    <ScrollView nestedScrollEnabled={true} style={{ maxHeight: 200 }}>
                      {countries.map((country, index) => (
                        <TouchableOpacity
                          key={index}
                          style={styles.countryItem}
                          onPress={() => {
                            setFormData({...formData, country});
                            setShowCountries(false);
                          }}
                        >
                          <Text style={styles.countryText}>{country}</Text>
                        </TouchableOpacity>
                      ))}
                    </ScrollView>
                  </View>
                )}

                <View style={styles.inputWrapper}>
                  <MaterialCommunityIcons name="phone" size={24} color="#666" style={styles.inputIcon} />
                  <TextInput
                    style={styles.input}
                    placeholder="Mobile Number"
                    keyboardType="phone-pad"
                    value={formData.mobile}
                    onChangeText={(text) => setFormData({...formData, mobile: text})}
                  />
                  {errors.mobile && <Text style={styles.errorText}>{errors.mobile}</Text>}
                </View>
              </>
            )}
          </View>

          <TouchableOpacity style={styles.submitButton} onPress={handleSubmit}>
            <Text style={styles.submitButtonText}>
              {isSignIn ? 'Sign In' : 'Sign Up'}
            </Text>
          </TouchableOpacity>

          {isSignIn && (
            <TouchableOpacity style={styles.forgotPassword}>
              <Text style={styles.forgotPasswordText}>Forgot Password?</Text>
            </TouchableOpacity>
          )}
        </View>
      </ScrollView>
    </KeyboardAvoidingView>
  );
}

const styles = StyleSheet.create({
  // ... (keep existing styles)
  
  // New Calculator Styles
  calculatorStep: {
    padding: 20,
  },
  stepTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 20,
  },
  stepSubtitle: {
    fontSize: 18,
    color: '#666',
    marginBottom: 10,
  },
  distanceInput: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: 'white',
    borderRadius: 10,
    padding: 15,
    marginBottom: 20,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  input: {
    flex: 1,
    fontSize: 18,
    color: '#333',
  },
  unitText: {
    fontSize: 16,
    color: '#666',
    marginLeft: 10,
  },
  frequencySection: {
    marginBottom: 20,
  },
  frequencyOptions: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    gap: 10,
  },
  frequencyButton: {
    paddingVertical: 10,
    paddingHorizontal: 15,
    borderRadius: 20,
    backgroundColor: '#E8F5E9',
    borderWidth: 1,
    borderColor: '#2E7D32',
  },
  frequencyButtonActive: {
    backgroundColor: '#2E7D32',
  },
  frequencyButtonText: {
    color: '#2E7D32',
    fontSize: 14,
    fontWeight: '600',
  },
  frequencyButtonTextActive: {
    color: 'white',
  },
  vehicleGrid: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    gap: 15,
    marginBottom: 30,
  },
  vehicleOption: {
    width: '48%',
    aspectRatio: 1,
    backgroundColor: '#E8F5E9',
    borderRadius: 15,
    justifyContent: 'center',
    alignItems: 'center',
    borderWidth: 1,
    borderColor: '#2E7D32',
  },
  vehicleOptionActive: {
    backgroundColor: '#2E7D32',
  },
  vehicleOptionText: {
    marginTop: 10,
    fontSize: 16,
    color: '#2E7D32',
    fontWeight: '600',
  },
  vehicleOptionTextActive: {
    color: 'white',
  },
  navigationButtons: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginTop: 20,
  },
  nextButton: {
    backgroundColor: '#2E7D32',
    paddingVertical: 15,
    borderRadius: 25,
    alignItems: 'center',
    marginTop: 20,
  },
  nextButtonDisabled: {
    opacity: 0.5,
  },
  nextButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  backButton: {
    flex: 1,
    backgroundColor: '#E8F5E9',
    paddingVertical: 15,
    borderRadius: 25,
    alignItems: 'center',
    marginRight: 10,
  },
  backButtonText: {
    color: '#2E7D32',
    fontSize: 18,
    fontWeight: 'bold',
  },
  calculateButton: {
    flex: 2,
    backgroundColor: '#2E7D32',
    paddingVertical: 15,
    borderRadius: 25,
    alignItems: 'center',
  },
  calculateButtonDisabled: {
    opacity: 0.5,
  },
  calculateButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  resultStep: {
    padding: 20,
  },
  resultTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 20,
    textAlign: 'center',
  },
  resultCard: {
    backgroundColor: 'white',
    borderRadius: 20,
    padding: 20,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  resultItem: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 20,
  },
  resultItemContent: {
    marginLeft: 15,
    flex: 1,
  },
  resultLabel: {
    fontSize: 14,
    color: '#666',
  },
  resultValue: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#333',
  },
  savingsCard: {
    backgroundColor: '#2E7D32',
    borderRadius: 15,
    padding: 15,
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 20,
  },
  savingsText: {
    color: 'white',
    marginLeft: 10,
    fontSize: 16,
    flex: 1,
  },
  impactSection: {
    marginTop: 20,
    padding: 15,
    backgroundColor: '#E8F5E9',
    borderRadius: 15,
  },
  impactTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 10,
  },
  impactText: {
    fontSize: 16,
    color: '#666',
    lineHeight: 24,
  },
  recalculateButton: {
    backgroundColor: '#2E7D32',
    paddingVertical: 15,
    borderRadius: 25,
    alignItems: 'center',
    marginTop: 20,
  },
  recalculateButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  modalContainer: {
    flex: 1,
    backgroundColor: 'rgba(0,0,0,0.5)',
    justifyContent: 'center',
    alignItems: 'center',
  },
  tipsModalContent: {
    backgroundColor: 'white',
    borderRadius: 20,
    width: '90%',
    maxHeight: '80%',
    padding: 20,
  },
  tipsHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  tipsTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  categoryScroll: {
    marginBottom: 20,
  },
  categoryButton: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#E8F5E9',
    paddingHorizontal: 15,
    paddingVertical: 10,
    borderRadius: 20,
    marginRight: 10,
  },
  selectedCategory: {
    backgroundColor: '#2E7D32',
  },
  categoryText: {
    marginLeft: 5,
    color: '#2E7D32',
    fontWeight: '600',
  },
  selectedCategoryText: {
    color: '#fff',
  },
  tipsContent: {
    flex: 1,
  },
  tipCard: {
    flexDirection: 'row',
    backgroundColor: '#fff',
    borderRadius: 15,
    padding: 15,
    marginBottom: 15,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  tipIconContainer: {
    width: 60,
    height: 60,
    backgroundColor: '#E8F5E9',
    borderRadius: 30,
    justifyContent: 'center',
    alignItems: 'center',
    marginRight: 15,
  },
  tipTextContainer: {
    flex: 1,
  },
  tipTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 5,
  },
  tipDescription: {
    fontSize: 14,
    color: '#666',
    marginBottom: 10,
  },
  impactContainer: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  impactText: {
    marginLeft: 5,
    color: '#2E7D32',
    fontWeight: '600',
  },  header: {
    flexDirection: 'row',
    justifyContent: 'flex-end',
    padding: 20,
    backgroundColor: 'transparent',
    zIndex: 1,
  },
  headerRight: {
    position: 'absolute',
    right: 20,
    top: 40,
  },
  darkModeHeader: {
    backgroundColor: '#1a1a1a',
  },
  settingsButton: {
    padding: 8,
    borderRadius: 20,
    backgroundColor: 'transparent',
  },  settingsModalContainer: {
    position: 'absolute',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    backgroundColor: 'rgba(0, 0, 0, 0.5)',
    justifyContent: 'flex-start',
    alignItems: 'flex-end',
    zIndex: 1000,
  },  settingsModalContent: {
    backgroundColor: 'white',
    borderRadius: 20,
    padding: 20,
    width: '80%',
    maxWidth: 300,
    marginTop: 80,
    marginRight: 20,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
  darkModeBackground: {
    backgroundColor: '#333',
  },
  settingsHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  settingsTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  darkModeText: {
    color: '#fff',
  },
  settingsOptions: {
    gap: 15,
  },
  settingsOption: {
    flexDirection: 'row',
    alignItems: 'center',
    padding: 15,
    backgroundColor: '#f5f5f5',
    borderRadius: 12,
  },
  darkModeOption: {
    backgroundColor: '#404040',
  },
  settingsIcon: {
    marginRight: 15,
  },
  settingsOptionText: {
    flex: 1,
    fontSize: 16,
    color: '#333',
  },
  logoutButton: {
    flexDirection: 'row',
    alignItems: 'center',
    padding: 15,
    backgroundColor: '#FF3B30',
    borderRadius: 12,
    marginTop: 10,
  },
  darkModeLogoutButton: {
    backgroundColor: '#cc2e25',
  },
  logoutButtonText: {
    flex: 1,
    fontSize: 16,
    color: '#fff',
    fontWeight: 'bold',
  },
  darkModeContainer: {
    backgroundColor: '#1a1a1a',
  },
  container: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  profileImage: {
    width: 100,
    height: 100,
    borderRadius: 50,
    marginBottom: 10,
  },
  modalContainer: {
    position: 'absolute',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    backgroundColor: 'rgba(0, 0, 0, 0.5)',
    justifyContent: 'center',
    alignItems: 'center',
  },
  modalContent: {
    backgroundColor: 'white',
    borderRadius: 20,
    padding: 20,
    width: '90%',
    maxHeight: '80%',
  },
  modalHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  modalTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  modalSubtitle: {
    fontSize: 16,
    color: '#666',
    marginBottom: 20,
  },
  inviteOptions: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    marginBottom: 30,
  },
  inviteOption: {
    width: '45%',
    backgroundColor: '#F5F5F5',
    padding: 15,
    borderRadius: 15,
    alignItems: 'center',
    marginBottom: 15,
  },
  inviteOptionLabel: {
    marginTop: 8,
    fontSize: 14,
    color: '#333',
  },
  referralSection: {
    backgroundColor: '#E8F5E9',
    padding: 15,
    borderRadius: 15,
  },
  referralTitle: {
    fontSize: 16,
    color: '#333',
    marginBottom: 10,
  },
  referralCode: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    backgroundColor: 'white',
    padding: 10,
    borderRadius: 10,
  },
  referralCodeText: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#2E7D32',
  },
  challengesContainer: {
    padding: 20,
  },
  challengesHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  challengesTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  filterButton: {
    padding: 10,
  },
  challengeCard: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
    marginBottom: 20,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  challengeHeader: {
    marginBottom: 15,
  },
  challengeTitleContainer: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 5,
  },
  challengeTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
  },
  challengeDescription: {
    fontSize: 14,
    color: '#666',
  },
  challengeStats: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 15,
  },
  challengeStat: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  challengeStatText: {
    marginLeft: 5,
    fontSize: 14,
    color: '#666',
  },
  progressContainer: {
    marginBottom: 15,
  },
  progressBar: {
    height: 8,
    backgroundColor: '#E0E0E0',
    borderRadius: 4,
    overflow: 'hidden',
    marginBottom: 5,
  },
  progressFill: {
    height: '100%',
    backgroundColor: '#4CAF50',
    borderRadius: 4,
  },
  progressText: {
    fontSize: 12,
    color: '#666',
    textAlign: 'right',
  },
  challengeActions: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 15,
  },
  challengeButton: {
    backgroundColor: '#2E7D32',
    paddingVertical: 10,
    paddingHorizontal: 20,
    borderRadius: 25,
    flex: 1,
    marginRight: 10,
  },
  challengeButtonJoined: {
    backgroundColor: '#E8F5E9',
    borderWidth: 1,
    borderColor: '#2E7D32',
  },
  challengeButtonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
    textAlign: 'center',
  },
  challengeButtonTextJoined: {
    color: '#2E7D32',
  },
  inviteButton: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#E8F5E9',
    paddingVertical: 10,
    paddingHorizontal: 15,
    borderRadius: 25,
  },
  inviteButtonDisabled: {
    backgroundColor: '#F5F5F5',
  },
  inviteButtonText: {
    color: '#2E7D32',
    marginLeft: 5,
    fontSize: 14,
    fontWeight: '600',
  },
  inviteButtonTextDisabled: {
    color: '#999',
  },
  completedChallenge: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'center',
    backgroundColor: '#FFF8E1',
    padding: 10,
    borderRadius: 25,
    flex: 1,
  },
  completedChallengeText: {
    color: '#FFA000',
    marginLeft: 5,
    fontSize: 16,
    fontWeight: 'bold',
  },  leaderboardToggle: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'center',
    padding: 10,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
    marginTop: 10,
  },
  leaderboardToggleText: {
    color: '#2E7D32',
    fontSize: 14,
    fontWeight: '600',
    marginRight: 5,
  },
  leaderboardContainer: {
    marginTop: 10,
    padding: 15,
    backgroundColor: '#F5F5F5',
    borderRadius: 10,
  },
  leaderboardTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 10,
  },
  leaderboardRow: {
    flexDirection: 'row',
    alignItems: 'center',
    paddingVertical: 8,
    borderBottomWidth: 1,
    borderBottomColor: '#E0E0E0',
  },
  userLeaderboardRow: {
    backgroundColor: '#E8F5E9',
    borderRadius: 8,
    marginVertical: 2,
  },
  leaderboardAvatar: {
    fontSize: 20,
    marginRight: 10,
  },
  leaderboardRank: {
    width: 40,
    fontSize: 14,
    fontWeight: 'bold',
    color: '#666',
  },
  leaderboardName: {
    flex: 1,
    fontSize: 14,
    color: '#333',
  },
  leaderboardPoints: {
    fontSize: 14,
    fontWeight: 'bold',
    color: '#2E7D32',
  },
  userLeaderboardText: {
    color: '#2E7D32',
    fontWeight: 'bold',
  },
  // ... existing styles ...
  calculatorContainer: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  calculatorHeader: {
    flexDirection: 'row',
    alignItems: 'center',
    padding: 20,
    backgroundColor: 'white',
    borderBottomWidth: 1,
    borderBottomColor: '#E0E0E0',
  },
  calculatorTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#333',
    marginLeft: 20,
  },
  calculatorContent: {
    padding: 20,
  },
  calculatorSection: {
    marginBottom: 20,
  },
  inputGroup: {
    gap: 10,
  },
  calculatorInput: {
    backgroundColor: 'white',
    padding: 15,
    borderRadius: 10,
    borderWidth: 1,
    borderColor: '#E0E0E0',
    fontSize: 16,
  },
  calculateButton: {
    backgroundColor: '#2E7D32',
    padding: 18,
    borderRadius: 25,
    alignItems: 'center',
    marginVertical: 20,
  },
  calculateButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  resultContainer: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 15,
    alignItems: 'center',
    marginBottom: 20,
  },
  resultTitle: {
    fontSize: 18,
    color: '#333',
    marginBottom: 10,
  },
  resultValue: {
    fontSize: 32,
    fontWeight: 'bold',
    color: '#2E7D32',
    marginBottom: 10,
  },
  resultComparison: {
    fontSize: 16,
    color: '#666',
  },
  autoCalculatorContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
  autoCalculatorText: {
    fontSize: 16,
    color: '#666',
    textAlign: 'center',
  },
  container: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  mainContainer: {
    flex: 1,
    backgroundColor: '#F5F5F5',
  },
  scrollView: {
    flex: 1,
  },
  header: {
    padding: 40,
    alignItems: 'center',
    borderBottomLeftRadius: 30,
    borderBottomRightRadius: 30,
  },
  headerTitle: {
    color: 'white',
    fontSize: 28,
    fontWeight: 'bold',
    marginTop: 10,
  },
  headerSubtitle: {
    color: 'white',
    fontSize: 16,
    opacity: 0.9,
  },
  formContainer: {
    padding: 20,
  },
  tabContainer: {
    flexDirection: 'row',
    backgroundColor: '#E8F5E9',
    borderRadius: 25,
    padding: 5,
    marginBottom: 20,
  },
  tab: {
    flex: 1,
    paddingVertical: 12,
    alignItems: 'center',
    borderRadius: 20,
  },
  activeTab: {
    backgroundColor: '#2E7D32',
  },
  tabText: {
    fontSize: 16,
    color: '#2E7D32',
    fontWeight: '600',
  },
  activeTabText: {
    color: 'white',
  },
  inputContainer: {
    gap: 15,
  },
  inputWrapper: {
    backgroundColor: 'white',
    borderRadius: 12,
    flexDirection: 'row',
    alignItems: 'center',
    borderWidth: 1,
    borderColor: '#E0E0E0',
  },
  inputIcon: {
    padding: 10,
  },
  input: {
    flex: 1,
    padding: 15,
    fontSize: 16,
    color: '#333',
  },
  placeholder: {
    color: '#666',
  },
  countryList: {
    backgroundColor: 'white',
    borderRadius: 12,
    marginTop: -10,
    marginBottom: 10,
    borderWidth: 1,
    borderColor: '#E0E0E0',
    zIndex: 1000,
  },
  countryItem: {
    padding: 15,
    borderBottomWidth: 1,
    borderBottomColor: '#E0E0E0',
  },
  countryText: {
    fontSize: 16,
    color: '#333',
  },
  submitButton: {
    backgroundColor: '#2E7D32',
    padding: 18,
    borderRadius: 25,
    marginTop: 30,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
  submitButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
    textAlign: 'center',
  },
  forgotPassword: {
    marginTop: 15,
    alignItems: 'center',
  },
  forgotPasswordText: {
    color: '#2E7D32',
    fontSize: 16,
  },
  errorText: {
    color: '#FF3B30',
    fontSize: 12,
    marginTop: 5,
    position: 'absolute',
    bottom: -20,
    left: 40,
  },
  // Welcome Quote Screen Styles
  quoteContainer: {
    flex: 1,
    backgroundColor: '#F5F5F5',
    padding: 20,
    justifyContent: 'center',
    alignItems: 'center',
  },
  closeButton: {
    position: 'absolute',
    top: 40,
    right: 20,
    padding: 10,
  },
  quote: {
    fontSize: 28,
    color: '#2E7D32',
    textAlign: 'center',
    fontWeight: 'bold',
    marginBottom: 20,
    lineHeight: 38,
  },
  quoteAuthor: {
    fontSize: 18,
    color: '#666',
    fontStyle: 'italic',
  },
  // Tab Bar Styles
  tabBar: {
    flexDirection: 'row',
    backgroundColor: 'white',
    paddingVertical: 10,
    paddingHorizontal: 20,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
  },
  tab: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  tabText: {
    fontSize: 12,
    color: '#666',
    marginTop: 4,
  },
  // Dashboard Styles
  tabContent: {
    flex: 1,
    padding: 20,
  },  // Dashboard Styles
  carbonCard: {
    borderRadius: 20,
    overflow: 'hidden',
    marginBottom: 20,
    elevation: 5,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
  },
  carbonGradient: {
    padding: 20,
  },
  carbonHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 20,
  },
  carbonTitle: {
    color: 'white',
    fontSize: 20,
    fontWeight: 'bold',
  },
  carbonContent: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'space-between',
  },
  carbonCircle: {
    width: 120,
    height: 120,
    borderRadius: 60,
    backgroundColor: 'rgba(255,255,255,0.2)',
    justifyContent: 'center',
    alignItems: 'center',
    borderWidth: 2,
    borderColor: 'white',
  },
  carbonValue: {
    color: 'white',
    fontSize: 36,
    fontWeight: 'bold',
  },
  carbonUnit: {
    color: 'white',
    fontSize: 14,
    opacity: 0.9,
  },
  carbonStats: {
    flex: 1,
    marginLeft: 20,
  },
  carbonStat: {
    marginBottom: 15,
  },
  carbonStatLabel: {
    color: 'white',
    fontSize: 14,
    opacity: 0.9,
  },
  carbonStatValue: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  achievementContainer: {
    marginBottom: 20,
  },
  achievementScroll: {
    marginTop: 10,
  },
  achievementCard: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 15,
    marginRight: 15,
    width: 150,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 3,
  },
  achievementIcon: {
    width: 50,
    height: 50,
    borderRadius: 25,
    backgroundColor: '#E8F5E9',
    justifyContent: 'center',
    alignItems: 'center',
    marginBottom: 10,
  },
  achievementTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 5,
  },  achievementDesc: {
    fontSize: 12,
    color: '#666',
    marginBottom: 5,
  },
  achievementDate: {
    fontSize: 10,
    color: '#999',
    fontStyle: 'italic',
  },
  quickActions: {
    marginBottom: 20,
  },
  actionGrid: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    marginTop: 10,
  },
  actionCard: {
    width: '48%',
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 15,
    alignItems: 'center',
    marginBottom: 15,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 3,
  },
  actionTitle: {
    marginTop: 10,
    fontSize: 16,
    color: '#333',
    fontWeight: '600',
  },
  tipsContainer: {
    marginBottom: 20,
  },  scrollContentContainer: {
    flexGrow: 1,
    paddingBottom: 20,
  },
  tipsScrollView: {
    maxHeight: '100%',
    width: '100%',
  },
  tipCard: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 15,
    marginBottom: 10,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 3,
    flexDirection: 'column',
    minHeight: 100,
    width: '100%',
  },
  completedTipCard: {
    backgroundColor: '#F1F8E9',
    borderColor: '#4CAF50',
    borderWidth: 1,
  },
  tipHeader: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 10,
  },
  tipIconContainer: {
    width: 40,
    height: 40,
    borderRadius: 20,
    backgroundColor: '#E8F5E9',
    justifyContent: 'center',
    alignItems: 'center',
    marginRight: 15,
  },  tipContent: {
    flex: 1,
    paddingRight: 10,
  },  tipTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 5,
    flexWrap: 'wrap',
  },  tipText: {
    fontSize: 14,
    color: '#666',
    flexWrap: 'wrap',
    marginBottom: 5,
  },  tipImpact: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginTop: 10,
    paddingTop: 10,
    borderTopWidth: 1,
    borderTopColor: '#E0E0E0',
  },
  impactContainer: {
    flexDirection: 'column',
    alignItems: 'flex-start',
  },
  pointsText: {
    color: '#4CAF50',
    fontSize: 14,
    fontWeight: 'bold',
    marginTop: 2,
  },
  completedText: {
    color: '#4CAF50',
    textDecorationLine: 'line-through',
    opacity: 0.8,
  },  impactText: {
    fontSize: 14,
    color: '#2E7D32',
    fontWeight: 'bold',
    flexWrap: 'wrap',
  },
  quickStats: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 20,
  },
  statCard: {
    flex: 1,
    backgroundColor: 'white',
    padding: 15,
    borderRadius: 15,
    alignItems: 'center',
    marginHorizontal: 5,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  statValue: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#2E7D32',
    marginVertical: 5,
  },
  statLabel: {
    fontSize: 12,
    color: '#666',
    textAlign: 'center',
  },
  aiSuggestionPreview: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 15,
  },
  tipCard: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#E8F5E9',
    padding: 15,
    borderRadius: 10,
  },
  tipText: {
    flex: 1,
    marginLeft: 15,
    fontSize: 16,
    color: '#333',
  },  // AI Recommendations Styles
  recommendationCard: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
    marginBottom: 15,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  categoryHeader: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 15,
  },
  recommendationTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#333',
    marginLeft: 10,
  },
  tipItem: {
    flexDirection: 'row',
    alignItems: 'center',
    paddingVertical: 12,
    borderBottomWidth: 1,
    borderBottomColor: '#E0E0E0',
  },
  completedTipItem: {
    backgroundColor: '#F1F8E9',
  },
  tipItemText: {
    flex: 1,
    marginLeft: 10,
    fontSize: 16,
    color: '#666',
  },
  completedTipText: {
    color: '#4CAF50',
    textDecorationLine: 'line-through',
  },
  pointsDisplay: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#E8F5E9',
    padding: 15,
    borderRadius: 10,
    marginBottom: 15,
  },
  pointsText: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#2E7D32',
    marginLeft: 10,
  },
  pointsBadge: {
    backgroundColor: '#E8F5E9',
    paddingHorizontal: 8,
    paddingVertical: 4,
    borderRadius: 12,
    marginLeft: 10,
  },
  pointsBadgeText: {
    color: '#2E7D32',
    fontSize: 12,
    fontWeight: 'bold',
  },
  // Profile Styles
  profileHeader: {
    alignItems: 'center',
    marginBottom: 30,
  },  profileName: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
    marginTop: 15,
  },
  profileBadge: {
    fontSize: 16,
    color: '#2E7D32',
    backgroundColor: '#E8F5E9',
    paddingHorizontal: 15,
    paddingVertical: 5,
    borderRadius: 15,
    marginTop: 5,
  },
  statsContainer: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginBottom: 30,
  },
  statItem: {
    alignItems: 'center',
  },
  statNumber: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#2E7D32',
  },
  leaderboardSection: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
  },
  leaderboardItem: {
    flexDirection: 'row',
    alignItems: 'center',
    paddingVertical: 10,
    borderBottomWidth: 1,
    borderBottomColor: '#E0E0E0',
  },
  userLeaderboardItem: {
    backgroundColor: '#E8F5E9',
    borderRadius: 10,
  },
  rankNumber: {
    width: 40,
    fontSize: 16,
    fontWeight: 'bold',
    color: '#666',
  },
  leaderboardName: {
    flex: 1,
    fontSize: 16,
    color: '#333',
  },
  leaderboardPoints: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#2E7D32',
  },
  // Rewards Styles
  rewardsHeader: {
    marginBottom: 20,
  },
  rewardsTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  rewardsPoints: {
    fontSize: 16,
    color: '#2E7D32',
    marginTop: 5,
  },
  rewardCard: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 20,
    marginBottom: 15,
    flexDirection: 'row',
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 3.84,
    elevation: 5,
  },
  rewardInfo: {
    flex: 1,
    marginLeft: 15,
  },
  rewardTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
  },
  rewardDescription: {
    fontSize: 14,
    color: '#666',
    marginTop: 5,
  },
  rewardPoints: {
    fontSize: 14,
    color: '#2E7D32',
    marginTop: 5,
  },
  claimButton: {
    backgroundColor: '#2E7D32',
    paddingHorizontal: 20,
    paddingVertical: 10,
    borderRadius: 20,
  },
  claimButtonText: {
    color: 'white',
    fontWeight: 'bold',
  },
});

const countries = [
  "United States", "United Kingdom", "Canada", "Australia", "Germany", "France", "India", 
  "Japan", "Brazil", "South Africa", "Nigeria", "Kenya", "China", "Singapore", "UAE"
];