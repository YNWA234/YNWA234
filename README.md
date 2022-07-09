# Website 
import json
from tkinter import *
import os

def read_json(filename):
  datafile = open(filename, 'r', encoding = 'utf-8')
  return json.loads(datafile.read())

global user_id

places = {}
user = {}
user_id = 0
current_user = None


def login(name):
  global current_user
  for user_id in user:
    if user[user_id].name == name:
      current_user = user_id
      break

def add_place(name):
  places[name] = Place(name)

def add_user(name, baseline):
  global user_id
  user[user_id] = Person(user_id, name, baseline)
  user_id += 1

def rate(place, food, rating):
  user[current_user].rate(places[place].menu[food], rating)


class Login:
    def __init__(self):
        self.root= Tk()
        canvas1 = Canvas(self.root, width=1450, height=900, bg='#afd0de')
        canvas1.pack()
        self.entry1 = Entry(self.root)
        self.entry2 = Entry(self.root)
        self.root.bind("<Return>", self.enter)
        canvas1.create_text(50, 127, text="Username", font=("Calibri", 20), anchor=NW)
        canvas1.create_text(50, 167, text="Password", font=("Calibri", 20), anchor=NW)
        canvas1.create_window(250, 140, window=self.entry1)
        canvas1.create_window(250, 180, window=self.entry2)

    def enter(self, event):
        print(self.entry1.get())
        for i in user:
            if user[i].name == self.entry1.get() and self.entry2.get() == "Password":
                self.root.destroy()
                window = Window(user[i].name)
                break
                

class Window:
    def __init__(self, user):
        global words
        self.root = Tk()
        self.user = user
        self.r = Canvas(self.root, width=1450, height=900, bg='#afd0de')
        self.r.pack()
        self.cart = []
        self.place = "McDonald's"#None
        self.r.create_rectangle(0, 0, 1450, 150, fill='#8facb8')
        #photo = PhotoImage(file = "/Users/bengbeng/Documents/McSpicy.png")
        #Label(self.root, text = 'GeeksforGeeks', font =('Verdana', 15)).pack(side = TOP, pady = 10)
        #photoimage = photo.subsample(3, 3)
        #Button(self.root, text = 'Click Me !', image = photoimage,
                    #compound = LEFT).pack(side = TOP)
        self.title = Label(self.root, text="Save Money Save Food", bg='#8facb8', font=("Calibri", 80), anchor=CENTER)
        self.title.place(x=300, y=20)
        self.user_name = Label(self.root, text=self.user, bg='#8facb8', font=("Calibri", 40), anchor=CENTER).place(x=1250, y=50)
        self.button1 = Button(self.root, command=lambda: self.add("McSpicy"), text="McSpicy\n" + str(places["McDonald's"].menu['McSpicy'].score), width=20, height=10)
        self.button1.place(x=100, y=200)
        self.button2 = Button(self.root, command=lambda: self.add('McChicken'), text="McChicken\n" + str(places["McDonald's"].menu['McChicken'].score), width=20, height=10)
        self.button2.place(x=430, y=200)
        self.button3 = Button(self.root, command=lambda: self.add('Buttermilk Crispy Chicken'), text="Buttermilk Crispy Chicken\n" + str(places["McDonald's"].menu['Buttermilk Crispy Chicken'].score), width=20, height=10)
        self.button3.place(x=760, y=200)
        self.button4 = Button(self.root, command=lambda: self.add('Filet-O-Fish'), text="Filet-O-Fish\n" + str(places["McDonald's"].menu['Filet-O-Fish'].score), width=20, height=10)
        self.button4.place(x=1090, y=200)
        self.button5 = Button(self.root, command=lambda: self.add("Angus BLT"), text="Angus BLT\n" + str(places["McDonald's"].menu['Angus BLT'].score), width=20, height=10)
        self.button5.place(x=100, y=400)
        self.button6 = Button(self.root, command=lambda: self.add('Chicken McNuggets (6pc)'), text="Chicken McNuggets (6pc)\n" + str(places["McDonald's"].menu['Chicken McNuggets (6pc)'].score), width=20, height=10)
        self.button6.place(x=430, y=400)
        self.button7 = Button(self.root, command=lambda: self.add('Big Mac'), text="Big Mac\n" + str(places["McDonald's"].menu['Big Mac'].score), width=20, height=10)
        self.button7.place(x=760, y=400)
        self.button8 = Button(self.root, command=lambda: self.add('Dbl McSpicy'), text="Dbl McSpicy\n" + str(places["McDonald's"].menu['Dbl McSpicy'].score), width=20, height=10)
        self.button8.place(x=1090, y=400)
        self.button5 = Button(self.root, command=lambda: self.add("Buttermilk Crispy Chicken"), text="Buttermilk Crispy Chicken\n" + str(places["McDonald's"].menu['Buttermilk Crispy Chicken'].score), width=20, height=10)
        self.button5.place(x=100, y=600)
        self.button6 = Button(self.root, command=lambda: self.add('Chicken McNuggets (9pc)'), text="Chicken McNuggets (9pc)\n" + str(places["McDonald's"].menu['Chicken McNuggets (9pc)'].score), width=20, height=10)
        self.button6.place(x=430, y=600)
        self.button7 = Button(self.root, command=lambda: self.add('Dbl Filet-O-Fish'), text="Dbl Filet-O-Fish\n" + str(places["McDonald's"].menu['Dbl Filet-O-Fish'].score), width=20, height=10)
        self.button7.place(x=760, y=600)
        self.button8 = Button(self.root, command=lambda: self.add('Cheeseburger'), text="Cheeseburger\n" + str(places["McDonald's"].menu['Cheeseburger'].score), width=20, height=10)
        self.button8.place(x=1090, y=600)
        self.clear_button = Button(self.root, command=self.clear_cart, text="Clear cart", width=5, height=2)
        self.clear_button.place(x=1290, y=820)
        self.cart_display = None
        self.full_display = None

    def add(self, food):
        total_score = places["McDonald's"].menu[food].score
        for j in self.cart:
            total_score += places["McDonald's"].menu[j].score
        if total_score > 30:
            return
        self.cart.append(food)
        self.r.delete(self.cart_display)
        self.r.delete(self.full_display)
        cart_items = ''
        for i in range(len(self.cart)):
            if i == len(self.cart) - 1:
                cart_items += self.cart[i]
            else:
                cart_items += self.cart[i] + " + "
        self.cart_display = self.r.create_text(80, 820, text=cart_items, font=("Calibri", 30), anchor=NW)
        self.full_display = self.r.create_text(1200, 820, text="= " + str(total_score), font=("Calibri", 30), anchor=NW)

    def clear_cart(self):
        self.r.delete(self.cart_display)
        self.r.delete(self.full_display)
        self.cart = []
        

class Place:
  def __init__(self, name):
    self.name = name
    self.menu = {}
        
  def add_food(self, food_name, food_score, base):
    self.menu[food_name] = Food(food_name, food_score, base)
        

class Person:
  def __init__(self, id_no, name, baseline):
    self.id_no = id_no
    self.name = name
    self.baseline = baseline

  def rate(self, food, rating):
    if food.base:
      self.baseline = (self.baseline + 10/rating * food.score)/2
      print(f"{self.name}'s new baseline is {round(self.baseline, 2)}")
    else:
      food.times_rated += 1
      food.total_score += rating * self.baseline/10
      food.score = food.total_score/food.times_rated
      print(f"{food.name}'s new score is {round(food.score, 2)}")


class Food:
  def __init__(self, name, score, base):
    self.name = name
    self.total_score = score
    self.base = base
    self.times_rated = 1
    self.score = score


#add macs
add_place("McDonald's")

places["McDonald's"].menu = {
"Angus BLT": Food("Angus BLT", 12, False),
"Big Mac": Food("Big Mac", 15, False),
"Buttermilk Crispy Chicken": Food("Buttermilk Crispy Chicken", 12, False),
"Chicken McNuggets (20pc)": Food("Chicken McNuggets (20pc)", 20, False),
"Chicken McNuggets (6pc)": Food("Chicken McNuggets (6pc)", 6, True),
"Chicken McNuggets (9pc)": Food("Chicken McNuggets (9pc)", 9, False),
"Dbl Cheeseburger": Food("Dbl Cheeseburger", 12, False),
"Dbl Filet-O-Fish": Food("Dbl Filet-O-Fish", 15, False),
"Dbl McSpicy": Food("Dbl McSpicy", 15, False),
"Filet-O-Fish": Food("Filet-O-Fish", 10, False),
"McChicken": Food("McChicken", 10, False),
"McSpicy": Food("McSpicy", 10, True),
"Original Angus Cheeseburger": Food("Original Angus Cheeseburger", 12, False),
"Box A": Food("Box A", 20, False),
"Box B": Food("Box B", 10, False),
"McWings (4pc)": Food("McWings (4pc)", 4, False),
"Grilled Chicken McWrap": Food("Grilled Chicken McWrap", 10, False),
"Cheeseburger": Food("Cheeseburger", 8, False),
"Hamburger": Food("Hamburger", 6, False)
}


#add users
add_user("Sam", 10)
add_user("Ryan", 12)
add_user("James", 15)
add_user("Jax", 30)

login_window = Login()
