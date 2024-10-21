Part 1: Data Structure Implementation (Grapgh Algorithm)
class User:
    def __init__(self, name):
        self.name = name
        self.friends = []

    def add_friend(self, friend):
        if friend not in self.friends:
            self.friends.append(friend)
            friend.friends.append(self)

class SocialNetwork:
    def __init__(self):
        self.users = {}

    def add_user(self, name):
        if name not in self.users:
            self.users[name] = User(name)
        return self.users[name]

    def add_friendship(self, name1, name2):
        user1 = self.add_user(name1)
        user2 = self.add_user(name2)
        user1.add_friend(user2)

# Part 2: Algorithm Implementation
def recommend_friends(network, target_name):
    if target_name not in network.users:
        return []

    target_user = network.users[target_name]
    recommendations = {}
    queue = [(friend, 1) for friend in target_user.friends]
    visited = set([target_user] + target_user.friends)

    while queue:
        current_user, distance = queue.pop(0)
        
        if distance == 2:
            if current_user not in recommendations:
                recommendations[current_user] = 0
            recommendations[current_user] += 1
        
        if distance < 2:
            for friend in current_user.friends:
                if friend not in visited:
                    visited.add(friend)
                    queue.append((friend, distance + 1))

    sorted_recommendations = sorted(recommendations.items(), key=lambda x: (-x[1], x[0].name))
    return [user.name for user, count in sorted_recommendations]

# Part 3: Testing/Usage
def main():
    # Create the social network
    network = SocialNetwork()

    # Add friendships as shown in the image
    network.add_friendship("Uncle Dude", "Jack")
    network.add_friendship("Uncle Dude", "Sara")
    network.add_friendship("Uncle Dude", "Emily")
    network.add_friendship("Sara", "Bob")

    # Test the recommendation system
    target_user = "Uncle Dude"
    recommendations = recommend_friends(network, target_user)
    print(f"Friend recommendations for {target_user}: {recommendations}")

    # Additional test cases can be added here

if __name__ == "__main__":
    main()
