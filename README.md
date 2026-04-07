rooms = [[101, "Single", 1000, "Available", "", 0],
         [102, "Double", 1500, "Available", "", 0],
         [103, "Deluxe", 2000, "Available", "", 0]]

payments = []

while True:
    print("\n1.Show  2.Book  3.Check-In  4.Check-Out  5.Refund  6.Payment History  7.Exit")
    ch = int(input("Enter choice: "))

    if ch == 1:
        print("\nNo\tType\tPrice\tStatus")
        for r in rooms:
            print(r[0], "\t", r[1], "\t", r[2], "\t", r[3])

    elif ch in [2, 3, 4]:
        no = int(input("Enter room number: "))
        found = False

        for r in rooms:
            if r[0] == no:
                found = True

                if ch == 2 and r[3] == "Available":
                    r[4] = input("Enter customer name: ")
                    r[5] = int(input("Enter days: "))
                    r[3] = "Booked"
                    advance = r[2] * r[5] * 0.2
                    print("Room booked!")
                    print("Advance Payment (20%) = Rs.", advance)
                    payments.append({"customer": r[4], "room": r[0], "type": "Advance", "amount": advance})

                elif ch == 3 and r[3] == "Booked":
                    r[3] = "Occupied"
                    print("Check-in successful!")

                elif ch == 4 and r[3] == "Occupied":
                    total = r[2] * r[5]
                    advance_paid = total * 0.2
                    balance = total - advance_paid

                    print("\n----- BILL -----")
                    print("Customer Name:", r[4])
                    print("Room Number:", r[0])
                    print("Room Type:", r[1])
                    print("Days Stayed:", r[5])
                    print("Total Amount   = Rs.", total)
                    print("Advance Paid   = Rs.", advance_paid)
                    print("Balance Due    = Rs.", balance)

                    print("\nSelect Payment Method:")
                    print("1. Cash  2. Card  3. UPI")
                    pm = int(input("Enter choice: "))
                    if pm == 1:
                        print("Payment of Rs.", balance, "received via Cash.")
                    elif pm == 2:
                        print("Payment of Rs.", balance, "received via Card.")
                    elif pm == 3:
                        print("Payment of Rs.", balance, "received via UPI.")
                    else:
                        print("Invalid payment method!")

                    payments.append({"customer": r[4], "room": r[0], "type": "Final Payment", "amount": total})
                    r[3], r[4], r[5] = "Available", "", 0
                    print("Check-out successful!")

                else:
                    print("Operation not possible!")

        if not found:
            print("Invalid room number!")

    elif ch == 5:
        print("\n----- REFUND -----")
        name = input("Enter customer name for refund: ")
        no = int(input("Enter room number: "))
        found = False

        for r in rooms:
            if r[0] == no and r[4] == name and r[3] == "Booked":
                total = r[2] * r[5]
                advance_paid = total * 0.2
                refund = advance_paid * 0.5
                print("Booking cancelled for", name)
                print("Advance Paid     = Rs.", advance_paid)
                print("Refund (50%)     = Rs.", refund)
                print("Cancellation Fee = Rs.", advance_paid - refund)
                payments.append({"customer": name, "room": r[0], "type": "Refund", "amount": refund})
                r[3], r[4], r[5] = "Available", "", 0
                print("Refund processed successfully!")
                found = True

        if not found:
            print("No booked room found for this customer!")

    elif ch == 6:
        print("\n----- PAYMENT HISTORY -----")
        if len(payments) == 0:
            prin
