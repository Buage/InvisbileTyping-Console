## Complete Recent Discord Quest
> [!Warning]
> This is against Discord's TOS, use this at your own risks.
>

How to use this script:
1. Open Discord (Browser or App)
2. Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd>
3. Go to `Console` tab
4. Paste the following code and press Enter. If it doesn't work, type `allow pasting` then retry
```js
// Function to add the toggle button to the toolbar
function addToggleButton() {
    const textBar = document.querySelector('.buttons__7ecff');
    if (!textBar) {
        console.error("Couldn't find the text bar.");
        return;
    }

    // Create the button
    const button = document.createElement('button');
    const img = document.createElement('img');
    img.style.width = '30px'; // Adjust image size as needed
    img.style.height = '30px';
    button.appendChild(img);

    // Add styles to the button
    button.style.backgroundColor = 'transparent';
    button.style.border = 'none';
    button.style.padding = '0';
    button.style.cursor = 'pointer';
    button.style.marginRight = '10px'; // Add right padding

    // Add an event listener to toggle the functionality
    let isBlocking = getCookie('typingBlockEnabled') === 'true';
    updateButtonState();
    button.addEventListener('click', function() {
        isBlocking = !isBlocking;
        setCookie('typingBlockEnabled', isBlocking);
        updateButtonState();
        toggleTypingBlock(isBlocking);
    });

    // Function to update the button state and image
    function updateButtonState() {
        if (isBlocking) {
            img.src = 'https://media.discordapp.net/attachments/1241660726855077928/1241660758987898900/Typing-Disabled.png?ex=664b0231&is=6649b0b1&hm=fde7786a70ec8e0580200020c7bb673a3eaea5794c30f7ba1a51b2db7019fd2b&=&format=webp&quality=lossless';
            img.alt = 'Typing Blocked';
        } else {
            img.src = 'https://media.discordapp.net/attachments/1241660726855077928/1241660759281242184/Typing-Enabled.png?ex=664b0231&is=6649b0b1&hm=4cc2f2d732ba1ad7c01b47d5244273ded804bf12eca8822aad96b50068bcd28b&=&format=webp&quality=lossless';
            img.alt = 'Typing Enabled';
        }
    }

    // Append the button to the text bar
    textBar.appendChild(button);
}

// Function to toggle "typing" requests blocking
function toggleTypingBlock(block) {
    if (block) {
        // Save the original send method if not already saved
        if (!XMLHttpRequest.prototype._originalSend) {
            XMLHttpRequest.prototype._originalSend = XMLHttpRequest.prototype.send;
        }
        
        // Redefine the send method to block "typing" requests
        XMLHttpRequest.prototype.send = function() {
            if (this._url && this._url.includes('/api/v9/channels/') && this._url.endsWith('/typing')) {
                console.log('Typing request blocked:', this._url);
                return;
            }
            return XMLHttpRequest.prototype._originalSend.apply(this, arguments);
        };

        // Redefine the open method to capture the request URL
        if (!XMLHttpRequest.prototype._originalOpen) {
            XMLHttpRequest.prototype._originalOpen = XMLHttpRequest.prototype.open;
        }
        XMLHttpRequest.prototype.open = function(method, url) {
            this._url = url;
            return XMLHttpRequest.prototype._originalOpen.apply(this, arguments);
        };
    } else {
        // Restore the original methods
        if (XMLHttpRequest.prototype._originalSend) {
            XMLHttpRequest.prototype.send = XMLHttpRequest.prototype._originalSend;
        }
        if (XMLHttpRequest.prototype._originalOpen) {
            XMLHttpRequest.prototype.open = XMLHttpRequest.prototype._originalOpen;
        }
    }
}

// Function to set a cookie
function setCookie(name, value, days) {
    let expires = '';
    if (days) {
        const date = new Date();
        date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
        expires = "; expires=" + date.toUTCString();
    }
    document.cookie = name + "=" + (value || "") + expires + "; path=/";
}

// Function to get the value of a cookie
function getCookie(name) {
    const nameEQ = name + "=";
    const ca = document.cookie.split(';');
    for (let i = 0; i < ca.length; i++) {
        let c = ca[i];
        while (c.charAt(0) === ' ') c = c.substring(1, c.length);
        if (c.indexOf(nameEQ) === 0) return c.substring(nameEQ.length, c.length);
    }
    return null;
}

// Function to reload the script after a URL change
function reloadScriptAfterURLChange() {
    const currentURL = window.location.href;
    let timeout;
    
    // Check for URL change every 100 milliseconds
    const intervalId = setInterval(function() {
        if (currentURL !== window.location.href) {
            clearInterval(intervalId); // Stop checking
            clearTimeout(timeout); // Clear previous timeout if any
            // Wait for 10 milliseconds before re-executing the script
            timeout = setTimeout(function() {
                executeScript();
            }, 10);
        }
    }, 100);
}

// Function to execute the script
function executeScript() {
    addToggleButton();
    reloadScriptAfterURLChange();
}

// Initial execution of the script
executeScript();

console.log('Script to add a typing block toggle button loaded.');
```
7. You can now toggle the option to disable Typing Indicator, when there is no red bar, this mean everyone can see you type, else nobody see you type

## FAQ

**Q: Ctrl + Shift + I doesn't work**

A: You can download the [ptb client](https://discord.com/api/downloads/distributions/app/installers/latest?channel=ptb&platform=win&arch=x64) or follow [this link](https://www.reddit.com/r/discordapp/comments/sc61n3/comment/hu4fw5x/) to enable DevTools
