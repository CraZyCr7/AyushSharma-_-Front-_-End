# AyushSharma_Front_End
# Assignment Solution
### Q1. Explain what the simple LIST component does?
Ans.
```
- Each object in the item array must have a parameter with the name "text" and the type string. It gets an array of objects called "items" as props.
- For each object in the array, a new component called "SingleListItem" is rendered using map iteration, and the values of the onClickHandler, text, index, and isSelected are sent as props.
- Each SingleListItem component has a unique index value that allows the user to recognise it when they click on it. 
- The value of selectedIndex is initially set to null and changed to the appropriate index value of the list item when the user clicks using the onClickHandler.
- The background colour of the list items may be altered by the isSelected props if isSelected is true, the highlighted list item will be green; if false, the background colour of all list items will be red prior to user interaction.
- The 'WrappedSingleListItem' is a memoized functional component that is intended to only render when the value of the isSelected properties changes. If the user double-clicks the list item, the component won't re-render.
- Using the 'WrappedSingleListItem' Component, another memoized functional component, the 'WrappedListComponent' creates a list of elements.
```

### Q2. What problems / warnings are there with code?
Ans.
```
- The onClickHandler property in WrappedSingleList Item is called incorrectly; it should be called as a callback to be triggered when the list item is clicked.
- Even if it's not an issue, the WrappedSingleListItem proptypes index and isSelected do not have a necessary flag. As these two parameters are essential for the code's correct operation, it would be a good idea to include a required bit so that programmers don't forget to supply them.
- Since selectedIndex is an integer, the value "null" supplied to it is incorrect since it should be assigned with a numeric value like -1 at the beginning to indicate that nothing is chosen.
- UseState is not utilised appropriately in the WrappedListComponent since the variable name is used before the function name.
- The useState hook in the WrappedListComponent should be used to initialise the value of selectedIndex with -1, making sure that no list item is chosen at the start of the component.
- The fact that we are sending the value of isSelected as selectedIndex, which will always be true anytime selectedIndex is not equal to zero, is another logical mistake.
- The definition of WrappedListComponent proptypes has a syntax mistake.
- The map function does not define the value of the unique key as a parameter for each child value.
```
### Q3.Please fix, optimize, and/or modify the component as much as you think is necessary.
Ans.
```
import React, { useState, useEffect, useMemo, memo} from 'react';
 import PropTypes from 'propTypes';

//single List Item
const WrappedSingleListItem = ({
     index, 
      isSelected,
      onClickHandler,
       text,
}) => (
   <li  
        style={{ backgroundColor: isSelected ? 'green' : 'red'}}
        onClick={() => onClickHandler(index)}
    >
        {text}
    </li>
 );

WrappedSingleListItem.propTypes = {
    index : PropTypes.number.isRequired,
    isSelected : PropTypes.bool.isRequired,
    onClickHandler: PropTypes.func.isRequired,
    text: PropTypes.string.isRequired,
};

const singleListItem = memo(WrappedSingleListItem);

//List Component

const WrappedListComponent = ({ 
     items,
}) => {

const [selectedIndex, setSelectedIndex] = useState(-1);

useEffect(() =>{
    setSelectedIndex(-1);
}, [items]);

const handleClick = index => {
    setSelectedIndex(index);
};

return(
    <ul style={{ textAlign: 'left' }}>
        {items && items.map((item, index) => (
            <singleListItem
            key = {index}
            onClickHandler={() => handleClick(index)}
            text = {item.text}
            index = {index}
            isSelected = {selectedIndex === index}
            />
        ))}
    </ul>
)
};

WrappedListComponent.propTypes = {
        items: PropTypes.arrayOf(
        PropTypes.shape({
            text : PropTypes.string.isRequired,
        })
    ).isRequired,
};

WrappedListComponent.defaultProps = {
    items : [],
 };

const List = memo(WrappedListComponent);

export default List;
```



