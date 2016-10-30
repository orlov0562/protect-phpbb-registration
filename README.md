# protect-phpbb-registration

Simple registration bot protection 

1) Append to the end of ucp_registration.html, before "include overall_footer.html"
```
<script>
    var el = document.getElementById('email');
    el.onfocus = function(){
        el.style.display = 'none';
        var container = document.createElement('div');
        container.innerHTML = '<input name="emai1" id="emai1" type="email" tabindex="2" size="25" maxlength="100" value="{EMAIL}" class="inputbox autowidth" title="{L_EMAIL_ADDRESS}" autocomplete="off">';
        el.parentNode.appendChild(container);
        setTimeout(function(){ document.getElementById('emai1').focus(); }, 250);
        document.forms["register"].onsubmit = function(){
            el.parentNode.removeChild(el);
        }
    };
</script>
```

2) Append at the header of ucp.php:
```
if (!empty($_GET['mode']) && $_GET['mode'] =='register' && !empty($_POST['submit'])) {
    if (empty($_POST['emai1'])) {
        $_POST['email'] = '';
    } else {
        $_POST['email'] = $_POST['emai1'];
    }
}
```

3) Remove cache /cache/*.php and /cache/twig/

** Pay attention that script append and uses field emai1 (with number 1 at the end) instead original email (with l character at the end)
