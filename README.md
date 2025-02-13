import json
import matplotlib.pyplot as plt
import os

# File to store workout data
data_file = "workout_data.json"

def load_data():
    """Load workout data from a file."""
    if os.path.exists(data_file):
        with open(data_file, "r") as file:
            return json.load(file)
    return {}

def save_data(data):
    """Save workout data to a file."""
    with open(data_file, "w") as file:
        json.dump(data, file, indent=4)

def log_workout():
    """Log a new workout session."""
    date = input("Enter date (YYYY-MM-DD): ")
    exercise = input("Enter exercise name: ")
    reps = int(input("Enter number of reps: "))
    sets = int(input("Enter number of sets: "))
    weight = float(input("Enter weight used (kg): "))

    data = load_data()
    if date not in data:
        data[date] = []
    data[date].append({
        "exercise": exercise,
        "reps": reps,
        "sets": sets,
        "weight": weight
    })
    
    save_data(data)
    print("Workout logged successfully!")

def view_progress():
    """Visualize workout progress using Matplotlib."""
    data = load_data()
    if not data:
        print("No workout data available.")
        return
    
    exercise = input("Enter exercise to view progress: ")
    dates = []
    weights = []
    
    for date, workouts in sorted(data.items()):
        for workout in workouts:
            if workout["exercise"].lower() == exercise.lower():
                dates.append(date)
                weights.append(workout["weight"])
    
    if not dates:
        print("No data available for this exercise.")
        return
    
    plt.plot(dates, weights, marker='o', linestyle='-')
    plt.xlabel("Date")
    plt.ylabel("Weight (kg)")
    plt.title(f"Progress for {exercise}")
    plt.xticks(rotation=45)
    plt.grid()
    plt.show()

def suggest_improvements():
    """Suggest workout improvements based on logged data."""
    data = load_data()
    if not data:
        print("No workout data available.")
        return
    
    exercise = input("Enter exercise to get suggestions: ")
    weights = []
    
    for workouts in data.values():
        for workout in workouts:
            if workout["exercise"].lower() == exercise.lower():
                weights.append(workout["weight"])
    
    if len(weights) < 2:
        print("Not enough data for suggestions.")
        return
    
    if weights[-1] > weights[-2]:
        print("Great job! Keep increasing the weight gradually.")
    else:
        print("Consider adjusting form, increasing reps, or adding more sets.")

def main():
    """Main function to run the console-based app."""
    while True:
        print("\nWorkout Tracker")
        print("1. Log Workout")
        print("2. View Progress")
        print("3. Suggest Improvements")
        print("4. Exit")
        
        choice = input("Enter your choice: ")
        if choice == "1":
            log_workout()
        elif choice == "2":
            view_progress()
        elif choice == "3":
            suggest_improvements()
        elif choice == "4":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
