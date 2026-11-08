# Experience-Based Testing — Airbnb Clone

**Application:** `airbnb-clone-ts`  
**Feature Tested:** Creating a Booking

## Step 1 — Error Guessing

Based on my experience with similar booking websites, I identified three possible failures:

1. **Missing check-out date:** The application might allow a user to create a booking without selecting a check-out date, resulting in an incomplete reservation.
2. **Overlapping reservations:** The application might allow two users to book the same property for overlapping dates, resulting in a double booking.
3. **Multiple confirmation clicks:** The application might create duplicate reservations or fail to process a booking correctly if the user clicks the confirmation button multiple times before receiving a response.

These are predictions based on my experience with booking websites, not confirmed defects.

## Step 2 — Exploratory Testing

### Testing Charter

Explore the booking creation feature to investigate how the application handles overlapping reservations when multiple users attempt to book the same property for the same dates.

### Exploratory Testing Notes

| What I Tried | What Happened | What I Decided to Try Next |
|---|---|---|
| Created a booking for Cozy Cabin on December 2, 2026, with 5 guests. | The booking appeared on the Bookings page, but the rental name was missing. | Try creating another booking for the same property and date. |
| Created a second booking for Cozy Cabin on December 2, 2026, with 2 guests. | The application accepted both bookings without displaying a conflict warning. | Refresh the page to check whether the reservations were saved. |
| Refreshed the Bookings page. | Both reservations disappeared. | Investigate how booking information is stored. |
| Reviewed `db.json` and `BookingContext.tsx`. | Bookings were stored in temporary React state rather than persistent storage. | Document the findings and identify additional checks. |

### Exploratory Testing Findings

**Finding 1 — Potential Double Booking**

The application accepted two bookings for the same property and date without displaying a conflict warning. This could result in double bookings if the property is intended to allow only one reservation per date.

**Finding 2 — Missing Rental Name**

The Bookings page displayed the reservation date and number of guests, but the rental name was missing.

**Finding 3 — Booking Persistence**

The reservations disappeared after refreshing the page. Reviewing `BookingContext.tsx` showed that bookings were stored in React state without persistent storage.

### Reflection

Exploratory testing helped me identify issues that were not immediately noticeable when creating a reservation. Although the bookings appeared after submission, further testing revealed problems with overlapping reservations, missing information, and data persistence.


## Step 3 — Checklist-Based Testing

| # | Checklist Item | Result | Notes |
|---|---|---|---|
| 1 | Verify that bookings remain available after refreshing the page. | Failed | Reservations disappear after refresh. |
| 2 | Verify that a booking date is required. | Passed | Displays "Please select this field." |
| 3 | Verify that at least 1 guest is required. | Passed | Rejects 0 guests with a validation message. |
| 4 | Verify that a rental property must be selected. | Failed | Creates a booking without selecting a rental. |
| 5 | Verify that past booking dates are rejected. | Failed | Accepts bookings for October 15, 2026. |

### Checklist Testing Summary

- **Total checks:** 5
- **Passed:** 2
- **Failed:** 3

The checklist testing revealed issues with booking persistence, rental selection validation, and past-date validation. The application correctly requires a booking date and a minimum of one guest.
## Step 4 — Acceptance Testing

## Step 4 — Acceptance Testing

### Acceptance Criterion 1 — Create a Valid Booking

**Given** I am on the Add Booking page and Cozy Cabin is available.

**When** I select Cozy Cabin, choose December 5, 2026, enter 2 guests, and click Confirm Booking.

**Then** the application should create the booking and display the correct rental name, date, and number of guests.

**Result:** Failed

**Notes:** The booking was created and displayed the correct date and number of guests, but the rental property name was missing.

### Acceptance Criterion 2 — Require Rental Selection

**Given** I am on the Add Booking page.

**When** I enter a valid date and 2 guests, leave the rental property unselected, and click Confirm Booking.

**Then** the application should prevent the booking from being created and display an error message asking the user to select a rental property.

**Result:** Failed

**Notes:** The application created the booking without requiring a rental property or displaying an error message.

### Acceptance Criterion 3 — Booking Persistence

**Given** I have successfully created a booking for Cozy Cabin.

**When** I navigate to the Bookings page and refresh the browser.

**Then** I should still be able to see my previously created booking.

**Result:** Failed

**Notes:** The reservation disappeared after refreshing the browser.

### Acceptance Testing Summary

| # | Acceptance Criterion | Result | Notes |
|---|---|---|---|
| 1 | Create a valid booking and display the correct details. | Failed | Rental name was missing. |
| 2 | Prevent bookings without a rental property. | Failed | Booking was created without a rental. |
| 3 | Keep bookings available after refreshing the page. | Failed | Reservation disappeared after refresh. |

**Total Acceptance Tests:** 3  
**Passed:** 0  
**Failed:** 3

### Reflection

Acceptance testing helped me evaluate the booking feature from the user's perspective. Although the application allows users to create reservations, I identified issues with displaying rental information, validating required fields, and saving bookings. These issues could affect the user's ability to complete and manage reservations successfully.