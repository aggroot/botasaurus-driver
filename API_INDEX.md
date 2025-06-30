# Botasaurus Driver API Index

This document provides a comprehensive index of the public API exposed by the Botasaurus Driver library. This serves as a reference for implementing the Botasaurus adapter in Bitapify.

**Last Updated**: Based on actual implementation in driver.py
**Note**: This index includes public methods (not prefixed with `_`) from the Driver and Element classes.

## Table of Contents
- [Core Classes](#core-classes)
  - [Driver Class](#driver-class)
  - [Element Class](#element-class)
- [Constants](#constants)
  - [Wait Constants](#wait-constants)
- [Exceptions](#exceptions)

## Core Classes

### Driver Class

#### Initialization & Lifecycle
```python
def __init__(self, **kwargs) -> None
    """Initialize Driver with options like headless, proxy, etc."""

def close(self) -> None
    """Close the browser and clean up resources."""
```

#### Navigation
```python
def get(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60) -> Tab
    """Navigate to URL and optionally wait for page load."""

def google_get(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, accept_google_cookies: bool = False, timeout=60) -> Tab
    """Navigate to URL via Google (for Cloudflare bypass)."""

def get_via(self, link: str, referer: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60) -> Tab
    """Navigate to URL with custom referer."""

def get_via_this_page(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60) -> Tab
    """Navigate to URL from current page context."""

def reload(self, js_to_run_before_new_document=None) -> Tab
    """Reload the current page."""

def open_link_in_new_tab(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60) -> Tab
    """Open URL in a new tab."""
```

#### Page Properties
```python
@property
def current_url(self) -> str
    """Get the current page URL."""

@property
def page_html(self) -> str
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
    """Wait for element to appear and return it (raises exception if not found)."""

def is_element_present(self, selector: str, wait: Optional[int] = Wait.SHORT) -> bool
    """Check if element is present."""

def get_element_with_exact_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> Optional[Element]
    """Get element with exact text match."""

def get_element_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> Optional[Element]
    """Get element containing text."""

def get_all_elements_with_exact_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> List[Element]
    """Get all elements with exact text."""

def get_all_elements_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> List[Element]
    """Get all elements containing text."""
```

#### Element Interaction - Basic
```python
def click(self, selector: str, wait: Optional[int] = Wait.SHORT, skip_move: bool = False) -> None
    """Click element."""

def scroll(self, selector: Optional[str] = None, by: int = 1000, smooth_scroll: bool = True) -> None
    """Scroll to element or by amount."""

def type(self, selector: str, text: str, wait: Optional[int] = Wait.SHORT) -> None
    """Type text into element."""

def clear(self, selector: str, wait: Optional[int] = Wait.SHORT) -> None
    """Clear input field."""
```

#### Form Interaction
```python
def get_input_by_label(self, label: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get input element by label text."""

def type_by_label(self, label: str, text: str, wait: Optional[int] = Wait.SHORT) -> None
    """Type text in input found by label."""

def select_option(self, selector: str, value: Optional[Union[str, List[str]]] = None, index: Optional[Union[int, List[int]]] = None, label: Optional[Union[str, List[str]]] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Select option in dropdown."""

def upload_file(self, selector: str, file_path: str, wait: Optional[int] = Wait.SHORT) -> None
    """Upload file to file input."""

def upload_multiple_files(self, selector: str, file_paths: List[str], wait: Optional[int] = Wait.SHORT) -> None
    """Upload multiple files."""
```

#### JavaScript Execution
```python
def run_js(self, script: str, args: Optional[Any] = None) -> Any
    """Execute JavaScript and return result."""
```

#### Waiting & Conditions
```python
def sleep(self, seconds: float) -> None
    """Wait for specified seconds."""

def wait_for_page_to_be(self, expected_url: Union[str, List[str]], wait: Optional[int] = 8, raise_exception: bool = True) -> bool
    """Wait for page URL to match expected."""

def prompt(self, text: str = "Press Enter to continue...") -> None
    """Show prompt and wait for user input."""
```

#### Screenshots & Recording
```python
def save_screenshot(self, filename: Optional[str] = None) -> str
    """Save screenshot to file."""

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
def delete_cookies(self) -> None
    """Clear all cookies."""

def clear_localstorage(self) -> None
    """Clear local storage."""

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

<<<<<<< Updated upstream
def detect_and_bypass_cloudflare(self) -> None
    """Detect and bypass Cloudflare protection."""

def enable_human_mode(self) -> None
    """Enable human-like behavior mode."""

def disable_human_mode(self) -> None
    """Disable human-like behavior mode."""
=======
def run_cdp_command(self, command: str) -> Any
    """Execute a Chrome DevTools Protocol command directly."""
>>>>>>> Stashed changes
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
```

#### Network Interception
```python
def before_request_sent(self, callback: Callable) -> None
    """Set callback for before request is sent."""

def after_response_received(self, callback: Callable) -> None
    """Set callback for after response is received."""
```

#### Downloads
```python
def download_file(self, url: str, filename: Optional[str] = None) -> None
    """Download file from URL."""
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
```

#### Form Elements
```python
def check_element(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Check checkbox."""

def uncheck_element(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Uncheck checkbox."""

def select_option(self, value: Optional[Union[str, List[str]]] = None, index: Optional[Union[int, List[int]]] = None, label: Optional[Union[str, List[str]]] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Select dropdown option."""
```

#### File Upload
```python
def upload_file(self, file_path: str) -> None
    """Upload file to input element."""

def upload_multiple_files(self, file_paths: List[str]) -> None
    """Upload multiple files."""
```

#### Iframe & Shadow DOM
```python
def select_iframe(self) -> 'IframeElement'
    """Select iframe element."""

def get_shadow_root(self, wait: Optional[int] = Wait.SHORT) -> 'ShadowRoot'
    """Get shadow root."""
```

#### Link & Image Methods
```python
def get_link(self, search: Optional[str] = None) -> Optional[Link]
    """Get first link matching search."""

def get_all_links(self, search: Optional[str] = None) -> List[Link]
    """Get all links matching search."""

def get_image_link(self, search: Optional[str] = None) -> Optional[str]
    """Get first image link matching search."""

def get_all_image_links(self, search: Optional[str] = None) -> List[str]
    """Get all image links matching search."""
```

#### Text Search Methods (Element class)
```python
def get_element_with_exact_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> Optional['Element']
    """Get child element with exact text."""

def get_element_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> Optional['Element']
    """Get child element containing text."""

def get_all_elements_with_exact_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> List['Element']
    """Get all child elements with exact text."""

def get_all_elements_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT, type=None) -> List['Element']
    """Get all child elements containing text."""
```

## Constants

### Wait Constants

```python
class Wait:
    SHORT = 4      # 4 seconds
    LONG = 8       # 8 seconds  
    VERY_LONG = 16 # 16 seconds
```

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

## Notes

1. Most methods accept an optional `wait` parameter using the `Wait` constants
2. Many methods have both synchronous behavior and optional async waiting
3. Element selection methods return `None` if not found (unless using `wait_for_*` variants which raise exceptions)
4. All Driver methods should be called on a Driver instance
5. The Driver class manages its own Chrome subprocess via CDP (Chrome DevTools Protocol)
6. Methods not prefixed with `_` are considered public API
7. Some methods have different signatures between Driver and Element classes (e.g., `type()` parameter order)