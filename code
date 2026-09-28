import re
import matplotlib.pyplot as plt

# ----------------------------
# Appliances with realistic daily kWh
# ----------------------------
APPLIANCES = {
    1: ("LED TV", 0.15, False),
    2: ("Ceiling Fan", 0.18, False),
    3: ("Tube Light / CFL / LED Bulb", 0.06, False),
    4: ("LED Bulb", 0.03, False),
    5: ("Mobile Charger / Small Device", 0.01, False),
    6: ("Laptop", 0.1, True),
    7: ("Washing Machine", 0.6, True),
    8: ("Single-Door Refrigerator", 0.7, False),
    9: ("Double-Door Refrigerator", 0.35, False),
    10: ("Mixer Grinder", 0.25, True),
    11: ("Air Conditioner", 1.5, True),
    12: ("Geyser", 2.0, True),
    13: ("WiFi Router", 0.01, False)
}

RATE = 5.8  # ₹ per kWh

# ----------------------------
# 1) Show appliances
# ----------------------------
print("\nAppliances List:")
for num, (name, _, _) in APPLIANCES.items():
    print(f"{num}. {name}")

# ----------------------------
# 2) Get appliance numbers
# ----------------------------
nums = input("\nEnter appliance numbers (comma separated): ")
nums = list(map(int, nums.replace(" ", "").split(",")))

selected = {}

# ----------------------------
# 3) Get details for each appliance
# ----------------------------
for num in nums:
    name, daily_kwh, ask_hours = APPLIANCES[num]
    print(f"\n{name}")

    if ask_hours:
        line = input("Enter: quantity,hours per day or minutes (h/m),days: ").lower().replace(" ", "")
        try:
            q, t, d = line.split(",")
            quantity = int(q)
            days = int(d)

            match = re.match(r"(\d+)(h|m)", t)
            value = int(match.group(1))
            hours = value if match.group(2) == "h" else value / 60
        except:
            print("Invalid input, skipping.")
            continue

    else:
        line = input("Enter: quantity,days: ").replace(" ", "")
        try:
            q, d = line.split(",")
            quantity = int(q)
            days = int(d)
            hours = None
        except:
            print("Invalid input, skipping.")
            continue

    selected[num] = [quantity, hours, days]

# ----------------------------
# 4) Calculations
# ----------------------------
print("\n\nCost Breakdown:")
total = 0
costs_by_num = {}
kwh_by_num = {}

for num, data in selected.items():
    quantity, hours, days = data
    name, daily_kwh, ask_hours = APPLIANCES[num]

    if ask_hours:
        monthly_kwh = daily_kwh * hours * days * quantity
    else:
        monthly_kwh = daily_kwh * days * quantity

    cost = monthly_kwh * RATE
    costs_by_num[num] = cost
    kwh_by_num[num] = monthly_kwh
    total += cost

    print(f"{name} ({quantity} qty): ₹{cost:.2f}")

# ----------------------------
# 5) Total and prediction
# ----------------------------
print("\nTotal cost: ₹{:.2f}".format(total))
predicted = total * 1.05
print("Next month (predicted): ₹{:.2f}".format(predicted))

# ----------------------------
# 6) Suggestions (only appliances entered)
# ----------------------------
ideal = float(input("\nEnter your ideal monthly amount: ₹"))
print("\nSuggestions:")

if total <= ideal:
    print("Within ideal limit 👍")
else:
    diff = total - ideal
    print(f"Reduce around ₹{diff:.2f}")
    print("Try reducing usage of:")
    # Suggest appliances with highest cost first
    for num, cost in sorted(costs_by_num.items(), key=lambda x: x[1], reverse=True):
        print("-", APPLIANCES[num][0])

# ----------------------------
# 7) SINGLE PIE CHART (merged)
# ----------------------------
names = [APPLIANCES[n][0] for n in kwh_by_num]
values = [kwh_by_num[n] for n in kwh_by_num]

plt.figure(figsize=(7, 7))
plt.pie(values, labels=names, autopct="%1.1f%%")
plt.title("Energy Consumption Distribution (kWh Share)")
plt.show()

print("\nThank you for using the Smart Energy Consumption Analyzer!")
