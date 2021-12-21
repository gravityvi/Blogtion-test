
# Xpath

Xpath is a locator strategy used to find elements on a page. XPath stands for XML path. XPath uses path expressions to select nodes from documents.

## Usage in Automation Scripts

XPath can be useful while locating dynamic elements or while cross-browser testing due to different browser behavior towards the DOM element. 

## Finding elements from Element

There are a lot of different approaches for this purpose in different frameworks. Nightwatch uses page objects to define elements under a section and Selenium provides methods to look for an target-element inside an element. So let's take an example of a simple page that looks like this.

```javascript
<div>
	<input/>
	<div id="container">
			<input id="input-username"/>
	</div> 
<div>
```

We need to find the inner input element using XPath. One of the simpler ways would be to use

`//input[id="input-username"]`  but what if the page looks like this.

```javascript
<div>
	<input/>
	<div id="container">
			<input/>
	</div> 
<div>
```

Selenium has functions like findElement from an element to suit this case.

```javascript

      // Get element with tag name 'div'
      let element = driver.findElement(By.css("div[id=container]"));

      // Get the element available with tag name 'input' inside element
      let inputElement = await element.findElements(By.css("input"));
```

You will get the inner input element as expected. But what if you decide to use xpath instead of css selector.

```javascript
			// Get element with tag name 'div'
      let element = driver.findElement(By.css("div[id=container]"));

      // Get all the elements available on the page with tag name input
      let inputElement = await element.findElements(By.xpath("//input"));
```

It completely disregards your code to start searching for an element under a div element and gives you all the input elements present on the page. Well, xpath works differently it has different annotations for searching. In this example, we are using `//` which means searching anywhere for the element. There is a useful page that discusses more such xpath expressions: [https://devhints.io/xpath](https://devhints.io/xpath). In this very example, we need to use `./` for desired results.

There is also a test in Selenium Repository which shows this the expected behavior while using xpath:

```javascript
@Test
  public void testFindingElementsOnElementByXPathShouldFindTopLevelElements() {
    driver.get(pages.simpleTestPage);
    WebElement parent = driver.findElement(By.id("multiline"));
    List<WebElement> allPs = driver.findElements(By.xpath("//p"));
    List<WebElement> children = parent.findElements(By.xpath("//p"));
    assertThat(allPs.size()).isEqualTo(children.size());
  }
```

Happy hacking 😄
