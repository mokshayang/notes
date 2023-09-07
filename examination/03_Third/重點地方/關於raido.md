```JS
    <input type="radio" name="color" value="red" disabled> Red
    <input type="radio" name="color" value="green"> green  
    <input type="radio" name="color" value="blue"> blue  
    <input type="radio" name="color" value="while" checked> while  
    <input type="radio" name="color" value="black"> black  
    <input type="radio" name="color" value="NO"> NO  
    <div>radio</div>
    <div>name 要相同</div>
    <div>value</div>
    <div>checked = 已選 </div>
    <div>disabled = 禁用 <input type="text" disabled>/div>
    
    let type = $("input[name=type]:checked").val();
    let val = (type == "date") ? $('#date').val() :$('#name').val();
```