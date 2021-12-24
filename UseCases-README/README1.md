'''
Sangjin Lee
'''

1) index.html

This is the html where user inputs are received through form and sent to status_result.html and flight_result.html.
It will initially display the forms and once they are submitted, it will display publically viewable information
on flights and status.

2) home.html

This html displays a home page once the user logs in. It exhibits a set of pictures and sidebar menus for the 
verified users.

3) login.html

This html displays a form with which a user can use to input login details. This also has a dropdown menu for the user
to choose if they are a staff or a customer.

4) search_flights.html

This is related to the use case "search for flights". A user can input flight details (location, date, trip type) to 
search for the flights they are looking for.

5) search_flights_result.html

Once the user inputs all the necessary details from search_flights.html they are redirected to this page that displays 
the information requested.

6) register.html

If a user does not have a registered login details, they can set it up through this page.

7) purchase.html

This html page displays a form where users can input their card details to purchase the tickets for the flights they 
chose on the previous page. 

8) purchase_result.html

This html page displays the purchased flight (ticket) details.

9) view_flight.html 

This html page displays the list of flight details related to either the customer or staff who is registered and logged in.

10) status_result.html

This html page displays the flights that are chosen from the public view page and their status

11) flight_result.html

This html page displays the flights that are chosen from the public view page (one-way / round-trip)


'''

Use cases and queries

'''


# Use cases: View Public Info, Search Flights
# This query takes in departure/arrival locations and departure date to output flight details for one-way

query = "select distinct * from (select airline_name, flight_number, departure_date, departure_time, departure_airport_code, arrival_airport_code, airplane_ID, arrival_date, arrival_time, base_price, flight_status, city as departure_city from flight join airport on departure_airport_code = airport_code) as F natural join (select flight_number, city as arrival_city from flight join airport on arrival_airport_code = airport_code) as S where (departure_city = '%s' or departure_airport_code = '%s') and (arrival_city = '%s' or arrival_airport_code = '%s') and departure_date = '%s'" % (d_city, d_code, a_city, a_code, outbound)


#-------------------------------------------------------------------------------------------------------#

# Use cases: View Public Info, Search Flights
# This query takes in departure/arrival locations and departure date to output flight details for round-trip

data = "select distinct * from (select airline_name, flight_number, departure_date, departure_time, departure_airport_code, arrival_airport_code, airplane_ID, arrival_date, arrival_time, base_price, flight_status, city as departure_city from flight join airport on departure_airport_code = airport_code) as F natural join (select flight_number, city as arrival_city from flight join airport on arrival_airport_code = airport_code) as S where (departure_city = '%s' or departure_airport_code = '%s') and (arrival_city = '%s' or arrival_airport_code = '%s') and departure_date = '%s'" % (d_city, d_code, a_city, a_code, outbound)

coming = "select distinct * from (select airline_name, flight_number, departure_date, departure_time, departure_airport_code, arrival_airport_code, airplane_ID, arrival_date, arrival_time, base_price, flight_status, city as departure_city from flight join airport on departure_airport_code = airport_code) as F natural join (select flight_number, city as arrival_city from flight join airport on arrival_airport_code = airport_code) as S where (departure_city = '%s' or departure_airport_code = '%s') and (arrival_city = '%s' or arrival_airport_code = '%s') and departure_date = '%s'" % (a_city, a_code, d_city, d_code, return_)

#-------------------------------------------------------------------------------------------------------#

# Use cases: View Public Info
# This query outputs flight details and status

query = "SELECT airline_name, flight_number, departure_date, arrival_date, flight_status FROM flight WHERE airline_name='%s' AND flight_number='%s' AND departure_date ='%s' AND arrival_date = '%s'" % (airline, flight_num, departure_date, arrival_date)

#-------------------------------------------------------------------------------------------------------#

# Use cases: login, register
# This query outputs user details when the inputs are username and password. This query is used to verify a user.

query = "SELECT * FROM customer WHERE customer_name='%s' AND customer_password='%s'" % (username, password)
query = "SELECT * FROM staff WHERE username='%s' AND staff_password='%s'" % (username, password)

#-------------------------------------------------------------------------------------------------------#

# Use cases: register
# This query inserts user details into the database (customer/staff).

query2 = "INSERT INTO customer (customer_email, customer_name, customer_password, customer_address, customer_phone_number, customer_passport_number, customer_experation, passport_country, customer_doB) VALUES ('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')"  % (email, username, password, address, phone, passport, passport_ex, passport_country, dob)
query2 = "INSERT INTO staff (username, airline_name, staff_password, staff_F_name, staff_L_name, staff_dofB) VALUES ('%s', '%s', '%s', '%s', '%s', '%s')" % (username, airline, password, f, l, dob)

#-------------------------------------------------------------------------------------------------------#

# Use cases: rate
# This query outputs customer will be able to rate and comment on their previous flights (for which he/she purchased tickets and already took that flight)
# A join between customer and ticket tables are used to check the condition given.

query = "select customer_email, airline_name, flight_number, departure_date, departure_time from customer natural join ticket where customer_name = '%s' and departure_date < CURRENT_DATE()" % (username)

#-------------------------------------------------------------------------------------------------------#

# Use cases: rate
# This query inserts into the customer_experience table the rating and comment as well as other user profile info.

query2 = "INSERT INTO customer_experience (customer_email, airline_name, flight_number, departure_date, departure_time, customer_experience_comment, customer_experience_rating) VALUES ('%s', '%s', '%s', '%s', '%s', '%s', '%s')"  % (row[idx]['customer_email'], row[idx]['airline_name'], row[idx]['flight_number'], row[idx]['departure_date'], row[idx]['departure_time'], comment, rating)

#-------------------------------------------------------------------------------------------------------#

# Use cases: view_flights
# This query outputs flight details for future flights of a designated customer

query = "select flight_number, airline_name, departure_airport_code, departure_date, departure_time, arrival_airport_code, arrival_date, arrival_time from flight natural join ticket natural join customer where customer_name = '%s' and departure_date > CURRENT_DATE()" % (username)

#-------------------------------------------------------------------------------------------------------#

# Use cases: view_flights
# This query outputs flight details for a specific airline
query = "SELECT * FROM flight WHERE airline_name = '%s'" % (airline)

#-------------------------------------------------------------------------------------------------------#

# Use cases: search_flights
# This query joins two tailored tables on flight in order to match names and code for both arrival and departure airports.
# This is to ensure that a user can either search for the name or the code when looking for flight details.

data = "select distinct * from (select airline_name, flight_number, departure_date, departure_time, departure_airport_code, arrival_airport_code, airplane_ID, arrival_date, arrival_time, base_price, flight_status, city as departure_city from flight join airport on departure_airport_code = airport_code) as F natural join (select flight_number, city as arrival_city from flight join airport on arrival_airport_code = airport_code) as S where (departure_city = '%s' or departure_airport_code = '%s') and (arrival_city = '%s' or arrival_airport_code = '%s') and departure_date = '%s'" % (d_city, d_code, a_city, a_code, outbound)

coming = "select distinct * from (select airline_name, flight_number, departure_date, departure_time, departure_airport_code, arrival_airport_code, airplane_ID, arrival_date, arrival_time, base_price, flight_status, city as departure_city from flight join airport on departure_airport_code = airport_code) as F natural join (select flight_number, city as arrival_city from flight join airport on arrival_airport_code = airport_code) as S where (departure_city = '%s' or departure_airport_code = '%s') and (arrival_city = '%s' or arrival_airport_code = '%s') and departure_date = '%s'" % (a_city, a_code, d_city, d_code, return_date)

#-------------------------------------------------------------------------------------------------------#

# Use cases: purchase_results
# This query selects flight details for a specific flight number

query1 = "select flight_number, airline_name, departure_date, departure_time, base_price from flight where flight_number = '%s'" % (flight)

#-------------------------------------------------------------------------------------------------------#

# Use cases: purchase_results
# This is to check whether or not the user has already bought the same flight ticket before

see = "select * from ticket where customer_email = '%s' and flight_number = '%s'" % (email, flight)

#-------------------------------------------------------------------------------------------------------#

# Use cases: purchase_results
# This is to insert into ticket table information about the purchase and customer details

query2 = "INSERT INTO ticket values ('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (ticket_id, email, r1[i]["flight_number"], r1[i]["airline_name"], r1[i]["departure_date"], r1[i]["departure_time"], r1[i]["base_price"], card_type, card_num, name, exp, today, str(time))