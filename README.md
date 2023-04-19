import datetime

date_string = "FOO0320"
month = date_string[-2:]
day = date_string[-4:-2]
year = "2023"
date_string = f"{day}-{month}-{year}"
date = datetime.datetime.strptime(date_string, "%d-%m-%Y").date()
formatted_date = date.strftime("%d-%b-%Y")
print(formatted_date)





import datetime

formatted_date = "20-Mar-2023"
date = datetime.datetime.strptime(formatted_date, "%d-%b-%Y").date()
next_day = date + datetime.timedelta(days=1)
formatted_next_day = next_day.strftime("%d-%b-%Y")
print(formatted_next_day)
