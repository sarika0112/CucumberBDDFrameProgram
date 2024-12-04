# CucumberBDDFrameProgram

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.CacheLookup;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {
    WebDriver ldriver;
    
    public LoginPage(WebDriver rDriver) {
    	ldriver =rDriver;
    	PageFactory.initElements( rDriver,this);
    }
    
    @FindBy(id ="Email")
    @CacheLookup
    WebElement txtEmail;
    
    @FindBy(id ="Password")
    @CacheLookup
    WebElement txtPassword;
    
    @FindBy(xpath = "//input[@value ='Log in']")
    @CacheLookup
    WebElement btnLogin;
    
    @FindBy(linkText = "Logout")
    @CacheLookup
    WebElement lnkLogOut;
    
    public void setUserName(String uname) {
    	txtEmail.clear();
    	txtEmail.sendKeys(uname);
    }
    public void setPasswprd(String pwd) {
    	txtEmail.clear();
    	txtEmail.sendKeys(pwd);
    }
    public void clickLogin() {
    	btnLogin.click();
    }
    public void clickLogout() {
    	lnkLogOut.click();
    }
  
}

package StepDefination;

import org.junit.Assert;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

import PageObject.LoginPage;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;

public class Steps {
	public WebDriver driver;
	public LoginPage lp;
	
	@Given("User Launch Chrome browser")
	public void user_launch_chrome_browser() {
		driver =new ChromeDriver();
	   lp= new LoginPage(driver);
	}

	@When("User opens URL {string}")
	public void user_opens_url(String url) {
		driver.get(url);
	}

	@When("User enters Email as {string} and password as {string}")
	public void user_enters_email_as_and_password_as(String email, String password) {
	   lp.setUserName(email);
	   lp.setPasswprd(password);
	}

	@When("Click on Login")
	public void click_on_login() {
	    lp.clickLogin();
	}

	@Then("Page Title should be {string}")
	public void page_title_should_be(String title) {
		if(driver.getPageSource().contains("Login was unsuccessful.")) {
			driver.close();
			Assert.assertTrue(false);
		}else {
			
			Assert.assertEquals(title, driver.getTitle());
		}
	}

	@When("User click on Log out link")
	public void user_click_on_log_out_link() throws InterruptedException {
	    lp.clickLogout();
	    Thread.sleep(3000);
	}

	@Then("close browser")
	public void close_browser() {
	   driver.quit();
	}


}
