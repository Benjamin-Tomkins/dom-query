# dom-query
```
var allOptions = $(); // define allOptions outside any function

// Create a MutationObserver to watch for changes to #secondary-dc
var observer = new MutationObserver(function() {
    // After the options are updated, clone them
    allOptions = $("#secondary-dc option").clone();
    console.log("Options updated", allOptions);
});

// Start observing #secondary-dc for changes
observer.observe($("#secondary-dc")[0], { childList: true });

$("#primary-dc").on("change", function () {
    console.log("Primary dropdown changed");
    if(allOptions.length > 0) {
        // Store selected value
        var selectedValue = $(this).val();
        console.log("Selected value", selectedValue);

        // Store current selected option in secondary dropdown
        var secondarySelectedValue = $("#secondary-dc").val();

        // Restore original options in the secondary dropdown
        var newOptions = allOptions.clone();
        newOptions.filter("option[value='" + selectedValue + "']").remove();

        // Disconnect the observer
        observer.disconnect();

        // Update the options in #secondary-dc
        $("#secondary-dc").empty().append(newOptions);

        // Reconnect the observer
        observer.observe($("#secondary-dc")[0], { childList: true });

        // Reselect the previous selected option in secondary dropdown if it is not the one selected in primary
        if (secondarySelectedValue !== selectedValue) {
            $("#secondary-dc").val(secondarySelectedValue);
        }
    } else {
        console.log("No options to restore");
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
