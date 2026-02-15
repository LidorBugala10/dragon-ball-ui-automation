
from playwright.sync_api import Page
import allure
class DragonBallWeb:
    def __init__(self, page:Page):
        self.page = page
        self.items = page.locator("//h2[contains(@class,'MuiTypography-h2')]")
        self.scroll_element = page.locator("//*[text()='Scroll Down for more characters']")
    @allure.title("verify scroll to buttom")
    @allure.description("This test verify scroll to buttom")
    def scroll_to_buttom(self):
        while(self.scroll_element.is_visible()):
            self.page.keyboard.down("PageDown")
    @allure.title("Verify scroll to bottom and print characters")
    @allure.description("This function scrolls to the bottom and logs all characters found on the page.")   
    def print_characters(self):
        print("\n") 
        for i,item in enumerate(self.items.all()):
            print(f"{i+1} - {item.inner_text()}")    
    @allure.title("Verify scroll to bottom and print characters")
    @allure.description("This function counts all characters found on the page after scrolling to the bottom.")
    def get_characters_count(self):
        return self.items.count()
		
import allure
import pytest
from playwright.sync_api import Playwright, expect
from playwright_automation.Lesson16.dragon_ball_page import DragonBallWeb
class TestDragonBall:
    @pytest.fixture(autouse = True,scope="class")
    def setup(self,playwright:Playwright):
        global browser,context,page,  dragon_ball_web
        browser = playwright.chromium.launch(headless= False,channel="chrome",slow_mo=1000)
        context = browser.new_context()
        page = context.new_page()
        page.goto("https://web.dragonball-api.com/")
        dragon_ball_web = DragonBallWeb(page)
        yield
        context.close()
        page.close()
    @allure.title("Test01 - Verify all characters from Dragon Ball")
    @allure.description("This test scrolls to the bottom of the Dragon Ball page, prints all characters, and verifies the total count is correct.")
    def test01(self):
        dragon_ball_web.scroll_to_buttom()
        dragon_ball_web.print_characters()
        assert dragon_ball_web.get_characters_count() == 58


        

        
