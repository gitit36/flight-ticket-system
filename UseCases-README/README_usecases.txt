### Tamer queries and use cases###

1) addflight:

Enables staff member to add flight to the airline they work in 

Query: "INSERT INTO flight VALUES(%s, %s, %s, %s, %s, %s, %s, %s, %s,%s,%s)"

2)View customers on a flight (part of view flight for staff)

Enables staff member to see customers on a certain flight

            cust_flights = '''SELECT 
                                    c.* 
                            FROM 
                                ticket t
                            left join
                                customer c
                            on 
                                t.customer_email=c.customer_email
                            WHERE 
                                t.airline_name = %s 
                                AND t.flight_number= %s'''

3)staffsearch (part of view flight for staff)

Enables staff member to search a flight based on range of dates, departure airport and arrival airport

Query: '''SELECT * FROM flight WHERE (airline_name = %s) AND (departure_date between %s and %s)
            AND departure_airport_code=%s AND arrival_airport_code=%s'''



4) Trackmyspending

Enables customer to track the total amount they spent based on a range of dates grouped by month and year

query= '''SELECT SUM(ticket_sold_price) as Spent FROM Ticket WHERE customer_email = %s 
            AND ticket_purchase_date between %s and  %s
            GROUP BY YEAR(ticket_purchase_date), MONTH(ticket_purchase_date)'''


5)flightstatus

Enables staff member to update flight status

quer= "UPDATE flight SET flight_status = %s WHERE flight_number = %s AND departure_date=%s"




6)addplane:

Enables staff member to add plane to the airline they work in 

query: "INSERT INTO airplane VALUES(%s, %s, %s)"
