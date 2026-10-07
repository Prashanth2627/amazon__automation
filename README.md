# amazon__automation

### Program:
```
from selenium import webdriver 

from selenium.webdriver.common.by import By 

from selenium.webdriver.support.ui import WebDriverWait 

from selenium.webdriver.support import expected_conditions as EC 

import random 

import time 

 

driver = webdriver.Chrome() 

wait = WebDriverWait(driver, 20) 

 

# 1. Open Amazon 

print("1. Opening Amazon...") 

driver.get("https://www.amazon.in/") 

driver.maximize_window() 

 

time.sleep(3) 

 

# 2. Search 

print("2. Searching product...") 

 

search_box = wait.until( 

    EC.presence_of_element_located((By.ID, "twotabsearchtextbox")) 

) 

 

search_box.send_keys("wireless mouse") 

 

driver.find_element( 

    By.ID, "nav-search-submit-button" 

).click() 

 

time.sleep(5) 

 

print("Search completed") 

print("URL:", driver.current_url) 

 

# 3. Get product links 

print("3. Finding products...") 

 

links = driver.find_elements( 

    By.XPATH, 

    "//a[contains(@href,'/dp/')]" 

) 

 

product_links = [] 

 

for link in links: 

 

    href = link.get_attribute("href") 

 

    if href and "/dp/" in href: 

 

        if href not in product_links: 

            product_links.append(href) 

 

print("Number of products:", len(product_links)) 

 

# 4. Select random product 

if len(product_links) == 0: 

 

    print("❌ No product links found") 

    print("Amazon may be showing CAPTCHA/login/robot verification.") 

 

    time.sleep(30) 

    driver.quit() 

 

else: 

 

    product = random.choice(product_links) 

 

    print("4. Random product selected") 

    print(product) 

 

    # 5. Open product 

    print("5. Opening product page...") 

 

    driver.get(product) 

 

    time.sleep(5) 

 

    print("Current URL:", driver.current_url) 

 

    # 6. Find Add to Cart 

    print("6. Looking for Add to Cart...") 

 

    try: 

 

        add_cart = wait.until( 

            EC.presence_of_element_located( 

                (By.ID, "add-to-cart-button") 

            ) 

        ) 

 

        driver.execute_script( 

            "arguments[0].scrollIntoView({block:'center'});", 

            add_cart 

        ) 

 

        time.sleep(2) 

 

        driver.execute_script( 

            "arguments[0].click();", 

            add_cart 

        ) 

 

        print("✅ Product added to cart") 

 

    except Exception as e: 

 

        print("❌ Add to Cart not found") 

        print(e) 

 

        time.sleep(30) 

        driver.quit() 

 

    else: 

 

        time.sleep(5) 

 

        # 7. Open Cart 

        print("7. Opening cart...") 

 

        try: 

 

            cart = wait.until( 

                EC.presence_of_element_located( 

                    (By.ID, "nav-cart") 

                ) 

            ) 

 

            driver.execute_script( 

                "arguments[0].click();", 

                cart 

            ) 

 

            print("✅ Cart opened") 

 

        except Exception as e: 

 

            print("❌ Cart not found") 

            print(e) 

 

            time.sleep(30) 

            driver.quit() 

 

        else: 

 

            time.sleep(5) 

 

            # 8. Checkout 

            print("8. Going to checkout...") 

 

            try: 

 

                checkout = wait.until( 

                    EC.presence_of_element_located( 

                        (By.NAME, "proceedToRetailCheckout") 

                    ) 

                ) 

 

                driver.execute_script( 

                    "arguments[0].click();", 

                    checkout 

                ) 

 

                print("✅ Checkout opened") 

 

            except Exception as e: 

 

                print("❌ Checkout button not found") 

                print(e) 

 

            print() 

            print("================================") 

            print("Reached checkout stage") 

            print("================================") 

 

            time.sleep(30) 

 

            driver.quit()
```

### Output:
<img width="775" height="390" alt="image" src="https://github.com/user-attachments/assets/6b9d3289-1238-4e61-910a-3edd3d240b3e" />
