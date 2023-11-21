# Nurul-Nabila-
INDIVIDUAL ASSIGNMENT 1 
import os

# Clear terminal for Windows
if os.name == 'nt':
    os.system('cls')
# Clear terminal for macOS/Linux
else:
    os.system('clear')

# Initial DSR threshold
dsr_threshold = 0.4

# List to store loan calculations
loan_calculations = []

def calculate_monthly_installment(principal, annual_interest_rate, loan_term_years):
    # Monthly interest rate
    monthly_interest_rate = annual_interest_rate / 12 / 100

    # Total number of monthly payments
    total_payments = loan_term_years * 12

    # Monthly installment formula
    monthly_installment = (principal * monthly_interest_rate) / (1 - (1 + monthly_interest_rate) ** -total_payments)

    return monthly_installment

def calculate_total_payment(monthly_installment, loan_term_years):
    # Total payment over the term of the loan
    total_payment = monthly_installment * loan_term_years * 12

    return total_payment

def calculate_dsr(monthly_income, total_monthly_debt_payments):
    # Debt Service Ratio (DSR) calculation
    dsr = total_monthly_debt_payments / monthly_income

    return dsr

def display_loan_details(loan_details):
    print("\nLoan Details:")
    print(f"Principal Amount: ${loan_details['principal']}")
    print(f"Annual Interest Rate: {loan_details['annual_interest_rate']}%")
    print(f"Loan Term: {loan_details['loan_term']} years")
    print(f"Monthly Income: ${loan_details['monthly_income']}")
    print(f"Other Monthly Debts: ${loan_details['other_monthly_debts']}")
    print(f"Monthly Installment: ${loan_details['monthly_installment']:.2f}")
    print(f"Total Payment over Loan Term: ${loan_details['total_payment']:.2f}")
    print(f"Debt Service Ratio (DSR): {loan_details['dsr']:.2%}")
    print(f"Eligibility: {'Eligible' if loan_details['eligible'] else 'Not Eligible'}")

def display_all_loans():
    # Display all previous loan calculations
    if not loan_calculations:
        print("\nNo previous loan calculations.")
    else:
        print("\nAll Previous Loan Calculations:")
        for idx, loan in enumerate(loan_calculations, 1):
            print(f"\nCalculation {idx}:")
            display_loan_details(loan)

def delete_loan_calculation():
    display_all_loans()
    if loan_calculations:
        try:
            idx_to_delete = int(input("Enter the index of the calculation to delete: "))
            if 1 <= idx_to_delete <= len(loan_calculations):
                deleted_calculation = loan_calculations.pop(idx_to_delete - 1)
                print(f"\nDeleted Calculation {idx_to_delete}:")
                display_loan_details(deleted_calculation)
            else:
                print("Invalid index. Please enter a valid index.")
        except ValueError:
            print("Invalid input. Please enter a valid index.")

def modify_dsr_threshold():
    global dsr_threshold
    try:
        new_threshold = float(input("Enter the new DSR threshold: "))
        dsr_threshold = new_threshold
        print(f"\nDSR threshold updated to {dsr_threshold}")
    except ValueError:
        print("Invalid input. Please enter a valid DSR threshold.")

def main():
    while True:
        print("\nMenu:")
        print("1. Calculate a new loan")
        print("2. Display all previous loan calculations")
        print("3. Delete a previous calculation")
        print("4. Modify DSR threshold")
        print("5. Exit")

        choice = input("Enter your choice (1, 2, 3, 4, or 5): ")

        if choice == '1':
            # Prompt user for loan details
            principal_amount = float(input("Enter the principal loan amount: "))
            annual_interest_rate = float(input("Enter the annual interest rate (in percentage): "))
            loan_term_years = int(input("Enter the loan term in years: "))
            monthly_income = float(input("Enter the applicant's monthly income: "))
            other_monthly_debts = float(input("Enter any other monthly financial commitments: "))

            # Calculate monthly installment
            monthly_installment = calculate_monthly_installment(principal_amount, annual_interest_rate, loan_term_years)

            # Calculate total payment over the loan term
            total_payment = calculate_total_payment(monthly_installment, loan_term_years)

            # Calculate total monthly debt payments
            total_monthly_debt_payments = monthly_installment + other_monthly_debts

            # Calculate DSR
            dsr = calculate_dsr(monthly_income, total_monthly_debt_payments)

            # Check eligibility
            eligible = dsr <= dsr_threshold

            # Store loan details in a dictionary
            loan_details = {
                'principal': principal_amount,
                'annual_interest_rate': annual_interest_rate,
                'loan_term': loan_term_years,
                'monthly_income': monthly_income,
                'other_monthly_debts': other_monthly_debts,
                'monthly_installment': monthly_installment,
                'total_payment': total_payment,
                'dsr': dsr,
                'eligible': eligible
            }

            # Display loan details
            display_loan_details(loan_details)

            # Store loan details in the list
            loan_calculations.append(loan_details)

        elif choice == '2':
            # Display all previous loan calculations
            display_all_loans()

        elif choice == '3':
            # Delete a previous calculation
            delete_loan_calculation()

        elif choice == '4':
            # Modify DSR threshold
            modify_dsr_threshold()

        elif choice == '5':
            # Exit the program
            print("\nExiting the program. Goodbye!")
            break

        else:
            print("\nInvalid choice. Please enter 1, 2, 3, 4, or 5.")

if __name__ == "__main__":
    main()
