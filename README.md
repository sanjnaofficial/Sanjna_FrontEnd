1. Explain what the simple List Component does.
This React **List Component** displays a list of items, where each item is represented as a **SingleListItem** component which is passed the item's **text** and its **index** within the list. We also pass isSelected a boolean value to determine if the SingleListItem is selected or not, and onClickHandler which will handle the list whenever an item in the list is clicked on. 
 The SingleListItem component can be selected by clicking on it. When an item is selected, its background color **changes from red to green**.

2. What problems / warnings are there with code?
The problems/warnings with the code are:
1.	**Intital Value in useState hook**: The setSelectedIndex useState hook is being called incorrectly. It should be passed an initial value, but it is not.
2.	**Proper destructing of useState hook**: Moreover, the useState hook returns two values selectedIndex (the current state) and setSelectedIndex (a settter function that updates the state) which are not properly ordered in the given code. The current state (selectedIndex) should be put before the function (setSelectedIndex) in the destructing the array like [selectedIndex, setSelectedIndex]. 
3.	**isSelected prop**: The isSelected prop of the SingleListItem component should be a boolean, but it is being passed the selectedIndex state value, which is a number.
4.	**The propTypes declaration**: The propTypes declaration for the items prop of the WrappedListComponent is incorrect, also shapeof property doesn’t exists. It should be PropTypes.arrayOf(PropTypes.shape({text: PropTypes.string.isRequired })) .

3. Please fix, optimize, and/or modify the component as much as you think is necessary.
// here is the code

```
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
    index,
    isSelected,
    onClickHandler,
    text
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ 
    items
}) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};

WrappedListComponent.defaultProps = {
  items: [
    {
        text: "SteelEye"
    },
    {
        text: "FrontEnd"
    },
    {
        text:"Sanjna"
    }
  ]
};

const List = memo(WrappedListComponent);

export default List;```
