# dom-query

```
var allOptions = $(); // define allOptions outside any function

function updateSecondaryDCOptions() {
    // Code to update #secondary-dc options...

    // After the options are updated, clone them
    allOptions = $("#secondary-dc option").clone();
}

$("#primary-dc").on("change", function () {
    if(allOptions.length > 0) {
        // Store selected value
        var selectedValue = $(this).val();

        // Store current selected option in secondary dropdown
        var secondarySelectedValue = $("#secondary-dc").val();

        // Restore original options in the secondary dropdown
        $("#secondary-dc").empty().append(allOptions.clone());

        // Find and remove the matching option in the secondary dropdown
        $("#secondary-dc option[value='" + selectedValue + "']").remove();

        // Reselect the previous selected option in secondary dropdown
        $("#secondary-dc").val(secondarySelectedValue);
    }
});


```
```
var allOptions = $();

// Create a MutationObserver instance
// Create a MutationObserver instance
var observer = new MutationObserver(function(mutations) {
    // If #secondary-dc and its options have been added, store all original options
    if ($("#secondary-dc option").length > 0) {
        allOptions = $("#secondary-dc option").clone();
        
        // Get the values of the options and log them
        var optionValues = allOptions.map(function() {
            return this.value;
        }).get();
        console.log(optionValues);
        
        observer.disconnect();  // Stop observing when we found the options
    }
});

// Configuration of the observer
var config = { childList: true, subtree: true };

// Start observing the document with the configured parameters
observer.observe(document, config);

$("#primary-dc").on("change", function () {
    if(allOptions.length > 0) {
        // Store selected value
        var selectedValue = $(this).val();

        // Store current selected option in secondary dropdown
        var secondarySelectedValue = $("#secondary-dc").val();

        // Restore original options in the secondary dropdown
        $("#secondary-dc").empty().append(allOptions.clone());

        // Find and remove the matching option in the secondary dropdown
        $("#secondary-dc option[value='" + selectedValue + "']").remove();

        // Reselect the previous selected option in secondary dropdown
        $("#secondary-dc").val(secondarySelectedValue);
    }
});

```
