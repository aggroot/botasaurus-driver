# Botasaurus Driver API Index

This document provides a comprehensive index of the public API exposed by the Botasaurus Driver library through its `__init__.py` file. This serves as a reference for implementing the Botasaurus adapter in Bitapify.

**Note**: This index only includes what is publicly exported via `botasaurus_driver.__init__.py`. Internal classes and methods not exposed in the public API are excluded.

## Table of Contents
- [Core Classes](#core-classes)
  - [Driver Class](#driver-class)
  - [Element Class](#element-class)
- [Constants](#constants)
  - [Wait Constants](#wait-constants)
- [Special Elements](#special-elements)
  - [BrowserTab](#browsertab)
  - [IframeElement](#iframeelement)
  - [IframeTab](#iframetab)
- [Exceptions](#exceptions)
- [CDP Module](#cdp-module)

## Core Classes

### Driver Class

#### Initialization & Lifecycle
```python
def __init__(self, **kwargs) -> None
    """Initialize Driver with options like headless, proxy, etc."""

def close(self) -> None
    """Close the browser and clean up resources."""

def quit(self) -> None
    """Alias for close()"""
```

#### Navigation
```python
def get(self, url: str, wait: Optional[int] = None) -> Union[Response, None]
    """Navigate to URL and optionally wait for page load."""

def google_get(self, url: str, wait: Optional[int] = None, accept_cookies: bool = True) -> Union[Response, None]
    """Navigate to URL via Google (for Cloudflare bypass)."""

def bing_get(self, url: str, wait: Optional[int] = None) -> Union[Response, None]
    """Navigate to URL via Bing."""

def get_via(self, url: str, referer: str, wait: Optional[int] = None) -> Union[Response, None]
    """Navigate to URL with custom referer."""

def reload(self) -> None
    """Reload the current page."""

def back(self) -> None
    """Navigate back in browser history."""

def forward(self) -> None
    """Navigate forward in browser history."""
```

#### Page Properties
```python
@property
def current_url(self) -> str
    """Get the current page URL."""

@property
def page_source(self) -> str
    """Get the page HTML source."""

@property
def title(self) -> str
    """Get the page title."""

@property
def user_agent(self) -> str
    """Get the current user agent string."""
```

#### Element Selection
```python
def select(self, selector: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Select single element by CSS selector."""

def select_all(self, selector: str, wait: Optional[int] = Wait.SHORT) -> List[Element]
    """Select all elements by CSS selector."""

def wait_for_element(self, selector: str, wait: Optional[int] = Wait.SHORT) -> Element
    """Wait for element to appear and return it."""

def is_element_present(self, selector: str, wait: Optional[int] = Wait.SHORT) -> bool
    """Check if element is present."""

def get_element_with_exact_text(self, text: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get element with exact text match."""

def get_element_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get element containing text."""

def get_elements_with_exact_text(self, text: str, wait: Optional[int] = Wait.SHORT) -> List[Element]
    """Get all elements with exact text."""

def get_elements_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT) -> List[Element]
    """Get all elements containing text."""

def get_element_by_id(self, id: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get element by ID."""

def get_element_or_create_by_id(self, id: str, wait: Optional[int] = Wait.SHORT) -> Element
    """Get element by ID or create if not found."""

def get_element_or_none_by_id(self, id: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get element by ID or None."""

def get_element_parent_by_id(self, id: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get parent element of element with ID."""
```

#### Element Interaction - Basic
```python
def click(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Click element."""

def scroll(self, selector: Optional[str] = None) -> None
    """Scroll to element."""

def type(self, text: str, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Type text into element."""

def clear(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Clear input field."""
```

#### Form Interaction
```python
def get_input_by_label(self, label: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get input element by label text."""

def type_in_input_by_label(self, label: str, text: str, wait: Optional[int] = Wait.SHORT) -> None
    """Type text in input found by label."""

def select_option_by_label(self, label: str, value: Optional[str] = None, index: Optional[int] = None, text: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Select option in dropdown found by label."""

def upload(self, selector: str, path: str, wait: Optional[int] = Wait.SHORT) -> None
    """Upload file to file input."""

def upload_multiple(self, selector: str, paths: List[str], wait: Optional[int] = Wait.SHORT) -> None
    """Upload multiple files."""
```

#### JavaScript Execution
```python
def run_js(self, script: str, args: Optional[Any] = None) -> Any
    """Execute JavaScript and return result."""

def execute_script(self, script: str, *args) -> Any
    """Execute JavaScript with arguments."""

def execute_file(self, filename: str) -> Any
    """Execute JavaScript from file."""

def add_script(self, script: str) -> None
    """Add script to page."""

def remove_data_disabled(self) -> None
    """Remove data-disabled attributes."""
```

#### Waiting & Conditions
```python
def wait(self, seconds: float) -> None
    """Wait for specified seconds."""

def sleep(self, seconds: float) -> None
    """Alias for wait()."""

def wait_for_page_to_be(self, expected_url: Union[str, List[str]], wait: Optional[int] = 8, raise_exception: bool = True) -> bool
    """Wait for page URL to match expected."""

def prompt(self, text: str = "Press Enter to continue...") -> None
    """Show prompt and wait for user input."""

def beep(self, frequency: int = 800, duration: int = 500) -> None
    """Play beep sound."""
```

#### Screenshots & Recording
```python
def save_screenshot(self, filename: Optional[str] = None) -> str
    """Save screenshot to file."""

def screenshot(self, filename: Optional[str] = None) -> str
    """Alias for save_screenshot()."""

def full_screenshot(self, filename: Optional[str] = None) -> str
    """Take full page screenshot."""

def start_recording(self, filename: Optional[str] = None) -> None
    """Start recording browser session."""

def save_recording(self, filename: Optional[str] = None) -> str
    """Save and stop recording."""
```

#### Tabs & Windows
```python
def get_tabs(self) -> List['BrowserTab']
    """Get all open tabs."""

def close_current_tab(self) -> None
    """Close current tab."""

def close_tab(self, tab: 'BrowserTab') -> None
    """Close specific tab."""

def switch_to_tab(self, tab: 'BrowserTab') -> 'BrowserTab'
    """Switch to specific tab."""

def open_tab(self, url: str, wait: Optional[int] = None) -> 'BrowserTab'
    """Open new tab with URL."""
```

#### Links & Images
```python
def get_links(self, search: Optional[str] = None) -> List[Link]
    """Get all links on page."""

def get_images(self) -> List[str]
    """Get all image URLs."""

def links(self, search: Optional[str] = None) -> List[Link]
    """Alias for get_links()."""
```

#### Page Manipulation
```python
def highlight(self, selector: str) -> None
    """Highlight element with border."""

def press(self, key: str) -> None
    """Press keyboard key."""

def write(self, text: str) -> None
    """Write text at current cursor position."""

def clear_cookies(self) -> None
    """Clear all cookies."""

def clear_localstorage(self) -> None
    """Clear local storage."""

def delete_cookies(self) -> None
    """Delete all cookies."""

def get_cookies(self) -> List[dict]
    """Get all cookies."""

def get_cookie(self, name: str) -> Optional[dict]
    """Get cookie by name."""

def set_cookie(self, cookie: dict) -> None
    """Set cookie."""

def delete_cookie(self, name: str) -> None
    """Delete cookie by name."""

def add_cookie(self, cookie: dict) -> None
    """Add cookie."""

def set_cookies(self, cookies: List[dict]) -> None
    """Set multiple cookies."""
```

#### Local Storage
```python
@property
def local_storage(self) -> LocalStorage
    """Access local storage object."""
```

#### Advanced Features
```python
def get_bot_detected(self) -> bool
    """Check if bot detection triggered."""

def is_bot_detected(self) -> bool
    """Alias for get_bot_detected()."""

def is_in_page(self, target: str, wait: Optional[int] = None, raise_error: bool = False) -> bool
    """Check if text/URL is in page."""

def is_page_fully_loaded(self) -> bool
    """Check if page fully loaded."""

def solve_captcha(self) -> None
    """Attempt to solve captcha."""

def block_images(self) -> None
    """Block image loading."""

def block_images_and_css(self) -> None
    """Block images and CSS."""

def calculate_loading_time(self) -> float
    """Get page loading time."""
```

#### Scrolling
```python
def scroll_to_bottom(self) -> None
    """Scroll to page bottom."""

def scroll_to_top(self) -> None
    """Scroll to page top."""

def scroll_by(self, x: int, y: int) -> None
    """Scroll by pixels."""

def short_random_sleep(self) -> None
    """Sleep for random short duration."""

def long_random_sleep(self) -> None
    """Sleep for random long duration."""

def sleep_forever(self) -> None
    """Sleep indefinitely."""
```

#### Debugging & Info
```python
def get_driver_version(self) -> str
    """Get Chrome driver version."""

def get_browser_version(self) -> str
    """Get Chrome browser version."""

def prompt(self, text: str = "Press Enter to continue...") -> None
    """Show prompt and wait."""
```

### Element Class

#### Properties
```python
@property
def text(self) -> str
    """Get element text content."""

@property
def html(self) -> str
    """Get element HTML."""

@property
def tag_name(self) -> str
    """Get element tag name."""

@property
def parent(self) -> Optional['Element']
    """Get parent element."""

@property
def children(self) -> List['Element']
    """Get child elements."""

@property
def all_parents(self) -> List['Element']
    """Get all parent elements."""

@property
def src(self) -> Optional[str]
    """Get src attribute."""

@property
def href(self) -> Optional[str]
    """Get href attribute."""

@property
def value(self) -> str
    """Get input value."""

@property
def attributes(self) -> dict
    """Get all attributes."""
```

#### Selection Methods
```python
def select(self, selector: str, wait: Optional[int] = Wait.SHORT) -> Optional['Element']
    """Select child element."""

def select_all(self, selector: str, wait: Optional[int] = Wait.SHORT) -> List['Element']
    """Select all child elements."""

def wait_for_element(self, selector: str, wait: Optional[int] = Wait.SHORT) -> 'Element'
    """Wait for child element."""

def is_element_present(self, selector: str, wait: Optional[int] = Wait.SHORT) -> bool
    """Check if child element present."""
```

#### Interaction Methods
```python
def click(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT, skip_move: bool = False) -> None
    """Click element."""

def type(self, text: str, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Type text into element."""

def clear(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Clear element."""

def focus(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Focus element."""

def set_value(self, value: str) -> None
    """Set element value."""

def set_text(self, text: str) -> None
    """Set element text."""
```

#### Attribute Methods
```python
def get_attribute(self, attribute: str) -> str
    """Get attribute value."""

def has_attribute(self, attribute: str) -> bool
    """Check if has attribute."""
```

#### Position & Movement
```python
def get_bounding_rect(self, absolute: bool = False) -> DictPosition
    """Get element position."""

def scroll_into_view(self) -> None
    """Scroll element into view."""

def scroll(self, by: int = 1000, smooth_scroll: bool = True) -> None
    """Scroll within element."""

def scroll_to_bottom(self, smooth_scroll: bool = True) -> None
    """Scroll to element bottom."""

def can_scroll_further(self) -> bool
    """Check if can scroll more."""
```

#### JavaScript Execution
```python
def run_js(self, script: str, args: Optional[Any] = None) -> Any
    """Run JavaScript on element."""

def remove(self) -> None
    """Remove element from DOM."""
```

#### Mouse Operations
```python
def move_mouse_here(self, is_jump: bool = False) -> None
    """Move mouse to element."""

def move_mouse_to_point(self, x: int, y: int, is_jump: bool = False) -> None
    """Move mouse to point."""

def click_at_point(self, x: int, y: int, skip_move: bool = False) -> None
    """Click at specific point."""

def drag_and_drop_to(self, to_point: Union[tuple[int, int], 'Element']) -> None
    """Drag element to destination."""
```

#### Screenshot
```python
def save_screenshot(self, filename: Optional[str] = None) -> str
    """Save element screenshot."""

def screenshot(self, filename: Optional[str] = None) -> str
    """Alias for save_screenshot()."""
```

#### Form Elements
```python
def check_element(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Check checkbox."""

def uncheck_element(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Uncheck checkbox."""

def select_option(self, selector: str, value: Optional[Union[str, List[str]]] = None, index: Optional[Union[int, List[int]]] = None, label: Optional[Union[str, List[str]]] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Select dropdown option."""
```

#### Utility Methods
```python
def get_element_at_point(self, x: int, y: int, child_selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> Optional['Element']
    """Get element at coordinates."""

def get_parent_which_satisfies(self, predicate: Callable[['Element'], bool]) -> Optional['Element']
    """Get parent matching condition."""

def get_parent_which_is(self, tag_name: str) -> Optional['Element']
    """Get parent with tag name."""

def get_shadow_root(self, wait: Optional[int] = Wait.SHORT) -> 'ShadowRoot'
    """Get shadow root."""
```

## Constants

### Wait Constants

```python
class Wait:
    SHORT = 4      # 4 seconds
    LONG = 8       # 8 seconds  
    VERY_LONG = 16 # 16 seconds
```

## Special Elements

### BrowserTab
Inherits from Driver with additional tab-specific properties:
```python
@property
def id(self) -> str
    """Tab ID."""

@property
def title(self) -> str
    """Tab title."""

@property
def url(self) -> str
    """Tab URL."""
```

### IframeElement
Special element for iframe handling with methods to access iframe content.

### IframeTab
Tab-like interface for iframe content manipulation.

## Exceptions

The following exceptions are exported and can be raised by various operations:

```python
# Base exception
class DriverException(Exception):
    """Base exception for all driver-related errors."""

# Browser and page exceptions
class ChromeException(DriverException):
    """Chrome browser-related errors."""

class PageNotFoundException(DriverException):
    """Page not found or navigation failed."""

class CloudflareDetectionException(DriverException):
    """Cloudflare bot detection triggered."""

class GoogleCookieConsentException(DriverException):
    """Google cookie consent dialog issues."""

# Element exceptions
class ElementWithSelectorNotFoundException(DriverException):
    """Element with given CSS selector not found."""

class ElementWithTextNotFoundException(DriverException):
    """Element with given text not found."""

class ElementInitializationException(DriverException):
    """Element could not be initialized."""

class DetachedElementException(DriverException):
    """Element is no longer attached to DOM."""

class ElementPositionNotFoundException(DriverException):
    """Element position could not be determined."""

class ElementPositionException(DriverException):
    """Element position-related error."""

# Form element exceptions
class InputElementForLabelNotFoundException(DriverException):
    """Input element for given label not found."""

class CheckboxElementForLabelNotFoundException(DriverException):
    """Checkbox element for given label not found."""

# Iframe exceptions
class IframeNotFoundException(DriverException):
    """Iframe not found."""

class ShadowRootClosedException(DriverException):
    """Shadow root is closed and cannot be accessed."""

# Screenshot exceptions
class ScreenshotException(DriverException):
    """Screenshot operation failed."""

class ElementScreenshotException(DriverException):
    """Element screenshot operation failed."""

class InvalidFilenameException(DriverException):
    """Invalid filename provided."""

# JavaScript exceptions
class JavascriptException(DriverException):
    """JavaScript execution error."""

class JavascriptSyntaxException(JavascriptException):
    """JavaScript syntax error."""

class JavascriptRuntimeException(JavascriptException):
    """JavaScript runtime error."""
```

## CDP Module

The `cdp` module is also exported for direct Chrome DevTools Protocol access:

```python
from botasaurus_driver import cdp
```

This provides low-level access to Chrome DevTools Protocol for advanced use cases.

## Notes

1. Most methods accept an optional `wait` parameter using the `Wait` constants
2. Many methods have both synchronous behavior and optional async waiting
3. Element selection methods return `None` if not found (unless using `wait_for_*` variants)
4. The `@` decorators in the original code are NOT part of the public API - they're internal implementation details
5. All Driver methods should be called on a Driver instance, not as decorators
6. The Driver class manages its own Chrome subprocess via CDP (Chrome DevTools Protocol)
7. Only the classes, methods, and exceptions listed in this document are part of the public API
