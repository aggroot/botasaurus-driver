# Botasaurus Driver API Index (Updated)

This document provides a comprehensive index of the public API exposed by the Botasaurus Driver library. This serves as a reference for implementing the Botasaurus adapter in Bitapify.

**Last Updated**: January 2025 - Added `use_current_tab` parameter and `create_new_tab` method
**Note**: This index reflects the actual implementation with two Element classes:
- CoreElement (in core/element.py) - Low-level implementation
- Element wrapper (in driver.py) - High-level API used by consumers

## Recent Changes

### January 2025 Updates
- **New Method**: `create_new_tab()` - Creates a new tab and navigates to URL without switching the driver's current tab
  - Preserves driver's current tab context for safe concurrent operations
  - Returns the newly created Tab instance for direct manipulation
  - Supports Cloudflare bypass, custom JavaScript injection, and wait options
- **Modified Methods**: Added `use_current_tab` parameter to navigation methods:
  - `get()` - Now supports `use_current_tab` parameter to control tab creation behavior
  - `google_get()` - Now supports `use_current_tab` parameter 
  - `get_via()` - Now supports `use_current_tab` parameter
- **New Internal Method**: `get_with_tab()` in Browser class - Navigates within existing tab
- **Bug Fix**: These changes address race conditions that occurred when creating virtual browser adapters concurrently

## Table of Contents
- [Core Classes](#core-classes)
  - [Driver Class](#driver-class)
  - [Element Class](#element-class)
- [Constants](#constants)
  - [Wait Constants](#wait-constants)
- [Exceptions](#exceptions)

## Core Classes

### Driver Class

The Driver class inherits from BrowserTab and provides browser automation capabilities.

#### Initialization & Lifecycle
```python
def __init__(self, **kwargs) -> None
    """Initialize Driver with options like headless, proxy, etc."""

def close(self) -> None
    """Close the browser and clean up resources."""
```

#### Navigation
```python
def get(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60, use_current_tab=True) -> Tab
    """Navigate to URL and optionally wait for page load.
    
    Args:
        link: URL to navigate to
        bypass_cloudflare: Whether to detect and bypass Cloudflare
        js_to_run_before_new_document: JavaScript to run before page load
        wait: Seconds to wait after navigation
        timeout: Timeout for page load
        use_current_tab: If True, navigates in current tab; if False, creates new tab
    """

def google_get(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, accept_google_cookies: bool = False, timeout=60, use_current_tab=True) -> Tab
    """Navigate to URL via Google (for Cloudflare bypass).
    
    Args:
        link: URL to navigate to
        bypass_cloudflare: Whether to detect and bypass Cloudflare
        js_to_run_before_new_document: JavaScript to run before page load
        wait: Seconds to wait after navigation
        accept_google_cookies: Whether to accept Google cookie consent
        timeout: Timeout for page load
        use_current_tab: If True, navigates in current tab; if False, creates new tab
    """

def get_via(self, link: str, referer: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60, use_current_tab=True) -> Tab
    """Navigate to URL with custom referer.
    
    Args:
        link: URL to navigate to
        referer: Referer URL to use
        bypass_cloudflare: Whether to detect and bypass Cloudflare
        js_to_run_before_new_document: JavaScript to run before page load
        wait: Seconds to wait after navigation
        timeout: Timeout for page load
        use_current_tab: If True, navigates in current tab; if False, creates new tab
    """

def get_via_this_page(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, timeout=60) -> Tab
    """Navigate to URL from current page context."""

def reload(self, js_to_run_before_new_document=None) -> Tab
    """Reload the current page."""

def open_link_in_new_tab(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, new_window=True, timeout=60) -> Tab
    """Open URL in a new tab.
    
    Args:
        link: URL to open
        bypass_cloudflare: Whether to detect and bypass Cloudflare
        js_to_run_before_new_document: JavaScript to run before page load
        wait: Seconds to wait after navigation
        new_window: Whether to open in new window vs new tab
        timeout: Timeout for page load
    """

def create_new_tab(self, link: str, bypass_cloudflare=False, js_to_run_before_new_document: str = None, wait: Optional[int] = None, new_window=True, timeout=60) -> Tab
    """Create a new tab and navigate to URL without switching driver's current tab.
    
    This method creates a new tab and navigates to the specified URL while preserving
    the driver's current tab context. This is particularly useful for concurrent operations
    where you need to create new tabs without affecting the driver's state.
    
    Args:
        link: URL to navigate to in new tab
        bypass_cloudflare: Whether to detect and bypass Cloudflare
        js_to_run_before_new_document: JavaScript to run before page load
        wait: Seconds to wait after navigation
        new_window: Whether to open in new window vs new tab
        timeout: Timeout for page load (0 means no wait for page load)
    
    Returns:
        Tab: The newly created tab instance
    
    Note:
        Unlike open_link_in_new_tab, this method preserves the driver's current tab,
        making it safe for concurrent operations.
    """
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
def page_text(self) -> str
    """Get the page text content."""

@property
def title(self) -> str
    """Get the page title."""

@property
def user_agent(self) -> str
    """Get the current user agent string."""

@property
def profile(self) -> Profile
    """Get the browser profile."""

@property
def is_human_mode_enabled(self) -> bool
    """Check if human mode is enabled."""
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

def count(self, selector: str, wait: Optional[int] = Wait.SHORT) -> int
    """Count elements matching selector."""
```

#### Element Interaction
```python
def click(self, selector: str, wait: Optional[int] = Wait.SHORT, skip_move: bool = False) -> None
    """Click element."""

def click_element_containing_text(self, text: str, wait: Optional[int] = Wait.SHORT, skip_move: bool = False) -> None
    """Click element containing specific text."""

def scroll(self, selector: Optional[str] = None, by: int = 1000, smooth_scroll: bool = True) -> None
    """Scroll to element or by amount."""

def type(self, selector: str, text: str, wait: Optional[int] = Wait.SHORT) -> None
    """Type text into element."""

def clear(self, selector: str, wait: Optional[int] = Wait.SHORT) -> None
    """Clear input field."""

def focus(self, selector: str, wait: Optional[int] = Wait.SHORT) -> None
    """Focus element."""

def set_value(self, selector: str, value: str, wait: Optional[int] = Wait.SHORT) -> None
    """Set element value directly."""

def set_text(self, selector: str, text: str, wait: Optional[int] = Wait.SHORT) -> None
    """Set element text content."""
```

#### Form Interaction
```python
def get_input_by_label(self, label: str, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get input element by label text."""

def type_by_label(self, label: str, text: str, wait: Optional[int] = Wait.SHORT) -> None
    """Type text in input found by label."""

def check_element_by_label(self, label: str, wait: Optional[int] = Wait.SHORT) -> None
    """Check checkbox by label."""

def uncheck_element_by_label(self, label: str, wait: Optional[int] = Wait.SHORT) -> None
    """Uncheck checkbox by label."""

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

def get_js_variable(self, variable_name: str) -> Any
    """Get JavaScript variable value."""

def run_on_new_document(self, script: str) -> None
    """Run JavaScript on new document load."""
```

#### Waiting & Conditions
```python
def sleep(self, seconds: float) -> None
    """Wait for specified seconds."""

def short_random_sleep(self) -> None
    """Sleep for random short duration (0.2-0.7s)."""

def long_random_sleep(self) -> None
    """Sleep for random long duration (1.5-3s)."""

def sleep_forever(self) -> None
    """Sleep indefinitely."""

def wait_for_page_to_be(self, expected_url: Union[str, List[str]], wait: Optional[int] = 8, raise_exception: bool = True) -> bool
    """Wait for page URL to match expected."""

def prompt(self, text: str = "Press Enter to continue...") -> None
    """Show prompt and wait for user input."""

def is_in_page(self, target: str, wait: Optional[int] = None, raise_error: bool = False) -> bool
    """Check if text/URL is in page."""
```

#### Screenshots
```python
def save_screenshot(self, filename: Optional[str] = None) -> str
    """Save screenshot to file."""

def save_element_screenshot(self, selector: str, filename: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> str
    """Save element screenshot."""
```

#### Tabs & Windows
```python
def switch_to_tab(self, tab: 'BrowserTab') -> 'BrowserTab'
    """Switch to specific tab."""

def open_in_devtools(self) -> None
    """Open browser developer tools."""
```

#### Cookies & Storage
```python
def delete_cookies(self) -> None
    """Clear all cookies."""

def delete_local_storage(self) -> None
    """Clear local storage."""

def delete_cookies_and_local_storage(self) -> None
    """Clear cookies and local storage."""

def get_cookies(self) -> List[dict]
    """Get all cookies."""

def get_cookies_dict(self) -> dict
    """Get cookies as dictionary."""

def get_local_storage(self) -> dict
    """Get local storage data."""

def get_cookies_and_local_storage(self) -> dict
    """Get cookies and local storage combined."""

def add_cookies(self, cookies: List[dict]) -> None
    """Add multiple cookies."""

def add_local_storage(self, items: dict) -> None
    """Add local storage items."""

def add_cookies_and_local_storage(self, data: dict) -> None
    """Add cookies and local storage from combined data."""

@property
def local_storage(self) -> LocalStorage
    """Access local storage object."""
```

#### Bot Detection & Cloudflare
```python
def is_bot_detected(self) -> bool
    """Check if bot detection triggered."""

def get_bot_detected_by(self) -> str
    """Get bot detection source."""

def is_bot_detected_by_cloudflare(self) -> bool
    """Check if detected by Cloudflare."""

def detect_and_bypass_cloudflare(self) -> None
    """Detect and bypass Cloudflare protection."""

def prompt_to_solve_captcha(self) -> None
    """Prompt user to solve captcha."""
```

#### Human Mode
```python
def enable_human_mode(self) -> None
    """Enable human-like behavior mode."""

def disable_human_mode(self) -> None
    """Disable human-like behavior mode."""
```

#### Network Control
```python
def block_images(self) -> None
    """Block image loading."""

def block_images_and_css(self) -> None
    """Block images and CSS."""

def block_urls(self, urls: List[str]) -> None
    """Block specific URLs."""

def before_request_sent(self, callback: Callable) -> None
    """Set callback for before request is sent."""

def after_response_received(self, callback: Callable) -> None
    """Set callback for after response is received."""

def collect_response(self, url_contains: str, response_type: str = None) -> Optional[dict]
    """Collect response matching criteria."""

def collect_responses(self, url_contains: str, response_type: str = None) -> List[dict]
    """Collect all responses matching criteria."""
```

#### Downloads
```python
def download_file(self, url: str, filename: Optional[str] = None) -> None
    """Download file from URL."""

def download_element_video(self, selector: str, filename: Optional[str] = None, wait_for_download: bool = True, duration: Optional[float] = None) -> str
    """Download video from element."""

def is_element_video_downloaded(self, selector: str) -> bool
    """Check if element video is downloaded."""
```

#### Advanced Features
```python
def run_cdp_command(self, command: str, params: dict = None) -> Any
    """Execute Chrome DevTools Protocol command."""

def grant_all_permissions(self) -> None
    """Grant all browser permissions."""

def allow_insecure_connections(self) -> None
    """Allow insecure HTTPS connections."""

def dom_enable(self) -> None
    """Enable DOM domain."""

def dom_disable(self) -> None
    """Disable DOM domain."""
```

#### Scrolling
```python
def scroll_to_bottom(self) -> None
    """Scroll to page bottom."""

def scroll_into_view(self, selector: str, wait: Optional[int] = Wait.SHORT) -> None
    """Scroll element into view."""

def can_scroll_further(self) -> bool
    """Check if page can scroll further."""
```

#### Element Information
```python
def get_text(self, selector: str, wait: Optional[int] = Wait.SHORT) -> Optional[str]
    """Get element text."""

def get_attribute(self, selector: str, attribute: str, wait: Optional[int] = Wait.SHORT) -> Optional[str]
    """Get element attribute."""

def get_all_attributes(self, selector: str, wait: Optional[int] = Wait.SHORT) -> Optional[dict]
    """Get all element attributes."""

def remove(self, selector: str, wait: Optional[int] = Wait.SHORT) -> None
    """Remove element from DOM."""
```

#### Mouse Operations
```python
def click_at_point(self, x: int, y: int, skip_move: bool = False) -> None
    """Click at specific coordinates."""

def move_mouse_to_point(self, x: int, y: int, is_jump: bool = False) -> None
    """Move mouse to coordinates."""

def move_mouse_to_element(self, selector: str, wait: Optional[int] = Wait.SHORT, is_jump: bool = False) -> None
    """Move mouse to element."""

def mouse_press(self, x: int, y: int) -> None
    """Press mouse button at coordinates."""

def mouse_release(self, x: int, y: int) -> None
    """Release mouse button at coordinates."""

def mouse_press_and_hold(self, x: int, y: int, release_condition: Callable, check_interval: float = 0.1) -> None
    """Hold mouse button until condition met."""

def drag_and_drop(self, from_selector: str, to_selector: str, wait: Optional[int] = Wait.SHORT) -> None
    """Drag from one element to another."""

def get_element_at_point(self, x: int, y: int, wait: Optional[int] = Wait.SHORT) -> Optional[Element]
    """Get element at coordinates."""
```

### Element Class

The Element class (wrapper in driver.py) represents a DOM element with rich interaction capabilities.

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

def select_iframe(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> 'Element'
    """Select iframe element."""
```

#### Interaction Methods
```python
def click(self, selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT, skip_move: bool = False) -> None
    """Click element or child."""

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
    """Move mouse to point relative to element."""

def move_mouse_to_element(self, selector: str, wait: Optional[int] = Wait.SHORT, is_jump: bool = False) -> None
    """Move mouse to child element."""

def click_at_point(self, x: int, y: int, skip_move: bool = False) -> None
    """Click at specific point relative to element."""

def drag_and_drop_to(self, to_point: Union[Tuple[int, int], 'Element']) -> None
    """Drag element to destination."""

def mouse_press(self, x: Optional[int] = None, y: Optional[int] = None) -> None
    """Press mouse button."""

def mouse_release(self, x: Optional[int] = None, y: Optional[int] = None) -> None
    """Release mouse button."""

def mouse_press_and_hold(self, x: int, y: int, release_condition: Callable, check_interval: float = 0.1) -> None
    """Hold mouse button until condition met."""
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

def select_option(self, selector: Optional[str] = None, value: Optional[Union[str, List[str]]] = None, index: Optional[Union[int, List[int]]] = None, label: Optional[Union[str, List[str]]] = None, wait: Optional[int] = Wait.SHORT) -> None
    """Select dropdown option."""
```

#### File Upload
```python
def upload_file(self, file_path: str) -> None
    """Upload file to input element."""

def upload_multiple_files(self, file_paths: List[str]) -> None
    """Upload multiple files."""
```

#### Shadow DOM
```python
def get_shadow_root(self, wait: Optional[int] = Wait.SHORT) -> 'Element'
    """Get shadow root element."""
```

#### Link & Image Methods
```python
def get_link(self, selector: Optional[str] = None, url_contains_text: Optional[str] = None, element_contains_text: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> Optional[Link]
    """Get first link matching criteria."""

def get_all_links(self, selector: Optional[str] = None, url_contains_text: Optional[str] = None, element_contains_text: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> List[Link]
    """Get all links matching criteria."""

def get_image_link(self, selector: Optional[str] = None, url_contains_text: Optional[str] = None, element_contains_text: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> Optional[str]
    """Get first image link matching criteria."""

def get_all_image_links(self, selector: Optional[str] = None, url_contains_text: Optional[str] = None, element_contains_text: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> List[str]
    """Get all image links matching criteria."""
```

#### Parent Navigation
```python
def get_parent_which_satisfies(self, predicate: Callable[['Element'], bool]) -> Optional['Element']
    """Find parent matching condition."""

def get_parent_which_is(self, tag_name: str) -> Optional['Element']
    """Find parent with specific tag."""
```

#### Element Location
```python
def get_element_at_point(self, x: int, y: int, child_selector: Optional[str] = None, wait: Optional[int] = Wait.SHORT) -> Optional['Element']
    """Get element at coordinates relative to this element."""
```

#### Video Operations
```python
def download_video(self, filename: Optional[str] = None, wait_for_download_completion: bool = True, duration: Optional[float] = None) -> str
    """Download video from element."""

def is_video_downloaded(self) -> bool
    """Check if video downloaded."""
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
7. The Element class wrapper provides a high-level API over the CoreElement implementation
8. Some methods support batch operations (e.g., selecting multiple options, uploading multiple files)