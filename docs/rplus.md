# R+

#### *def randomgen1(path_to_rplus: str, low: int, high: int, more_random_but_slow=False, return_speed=False)*

The first version of the random generator.  

path_to_rplus - The path to where rplus is downloaded.  
low - The lowest point the random generator can go.  
high - The highest point the random generator can go.  
more_random_but_slow - If this is set to true, the random generator will make the number even more random,  
but it will be slower than usual.  
return_speed - If this is set to true, randomgen1 will return a list, with the speed and the number in it.  

#### *def randomgen2(path_to_rplus: str, return_speed=False)*

The second version of the random generator.  

path_to_rplus - The path to where rplus is downloaded.  
return_speed - If this is set to true, randomgen2 will return a list, with the speed and the number in it.  

#### *def randompassword(path_to_rplus: str, numberofletters=12, return_speed=False)*

A generator to generate secure, random passwords.  

path_to_rplus - The path to where rplus is downloaded.  
numberofletters - The number of letters the password has.  
return_speed - If this is set to true, randompassword will return a list, with the speed and the number in it.  

#### *def randomwordgen1(numberofletters: int, return_speed=False)*

The first version of a random word generator.  

numberofletters - The number of letters the word is supposed to have.  
return_speed - If this is set to true, randomgen1 will return a list, with the speed and the number in it.  

#### *def randomwordgen2(retunr_speed=False)*

The first version of a random word generator.  

return_speed - If this is set to true, randomgen1 will return a list, with the speed and the number in it.
