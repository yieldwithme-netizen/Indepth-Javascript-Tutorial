# jQuery

## Definition

jQuery is a **fast, small JavaScript library** for DOM manipulation and event handling.

## Basic Usage

```javascript
// Select elements
$('#myId').hide();
$('.myClass').show();

// Events
$('#btn').click(function() {
    $(this).toggleClass('active');
});

// AJAX
$.ajax({
    url: '/api/data',
    success: function(data) {
        console.log(data);
    }
});
```

## Quick Revision

- jQuery = DOM manipulation library
- `$()` to select elements
- Event handling
- AJAX requests
- Use modern JS instead

---

## Related Topics

- [[What-is-jQuery]] - [[What-is-jQuery|jQuery]]
- [[jQuery]] - [[jQuery|jQuery]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
- [[What-is-Event]] - [[What-is-Event|Events]]
