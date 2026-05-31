# What-is-jQuery

## Definition

jQuery is a **fast JavaScript library** for DOM manipulation.

## Example

```javascript
// Select
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
- $() to select elements
- Event handling
- AJAX requests
- Use modern JS instead

---

## Related Topics

- [[What-is-jQuery]] - [[What-is-jQuery|jQuery]]
- [[What-is-jQuery]] - [[What-is-jQuery|jQuery]]
- [[jQuery]] - [[jQuery|jQuery]]
- [[What-is-DOM]] - [[What-is-DOM|DOM]]
