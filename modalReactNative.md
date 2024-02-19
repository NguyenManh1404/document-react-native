# Use Modal In React Native



<details>
    <summary><b>react-native-modal: (auto background)</b></summary>

```js
import React from 'react';
import { StyleSheet } from 'react-native';
import Modal from 'react-native-modal';
import { WView, WHeaderPage, WText, WTouchable } from '@pikthat/pikthat-mobile-shared-component';
import { Gender } from '../../graphql/generated/graphql';
import { Colors } from '../../themes';

interface ModalGenderProps {
  value: string;
  isVisible: boolean;
  onClosedModal: () => void;
  onSelected: (value: string) => void;
}

const ModalGender = ({ isVisible, value, onClosedModal, onSelected }: ModalGenderProps) => {
  return (
    <Modal
      isVisible={isVisible}
      onSwipeComplete={onClosedModal}
      onBackdropPress={onClosedModal}
      onBackButtonPress={onClosedModal}
      style={[styles.modal]}
    >
      <WView h={250} color={Colors.white} borderTopLeftRadius={12} borderTopRightRadius={12}>
        <WHeaderPage
          iconLeft="ic-close-without-circle"
          isHeaderPopup
          title={'Gender'}
          onPressBack={onClosedModal}
        />
        <WView fill mTop={16} pHoz={16}>
          {[Gender.Male, Gender.Female].map((text: string, index: number) => (
            <WTouchable
              h={44}
              borderRadius={4}
              w="100%"
              key={`i-${index}`}
              justifyCenter
              alignCenter
              mTop={4}
              color={text === value ? Colors.primary1 : Colors.blackB5}
              onPress={() => onSelected(text)}
            >
              <WText type="medium14" color={text === value ? Colors.primary : Colors.blackB90}>
                {text}
              </WText>
            </WTouchable>
          ))}
        </WView>
      </WView>
    </Modal>
  );
};

export default ModalGender;

const styles = StyleSheet.create({
  modal: {
    justifyContent: 'flex-end',
    margin: 0,
  },
});


// using

  const [isVisibleGender, setVisibleGender] = useState<boolean>(false);
 <ModalGender
        isVisible={isVisibleGender}
        value={formik.values.gender}
        onClosedModal={onClosedGender}
        onSelected={(val: string) => {
          formik.setFieldValue('gender', val);
          setVisibleGender(false);
        }}
      />


```
</details>




<details>
    <summary><b>import { StyleSheet, Modal } from 'react-native'</b></summary>

```js
import React from 'react';
import { StyleSheet, Modal } from 'react-native';
import { WView, WText, WTouchable } from '@pikthat/pikthat-mobile-shared-component';
import { Colors } from '../../themes';
import { Screen } from '@pikthat/pikthat-mobile-shared-component/src/constants';

interface ModalPermissionLocationProps {
  isVisible: boolean;
  onClosedModal: () => void;
  text: string;
}

const ModalPermissionLocation = ({
  isVisible,
  onClosedModal,
  text,
}: ModalPermissionLocationProps) => {
  return (
    <Modal
      animationType="fade"
      transparent={true}
      onRequestClose={onClosedModal}
      visible={isVisible}
    >
      <WTouchable style={styles.modalContainer} activeOpacity={1} onPress={onClosedModal}>
        <WView style={styles.modalView}>
          <WText color={Colors.white} textAlign="center" style={styles.modalMessage}>
            {text}
          </WText>
        </WView>
      </WTouchable>
    </Modal>
  );
};

export default ModalPermissionLocation;

const styles = StyleSheet.create({
  modalContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  modalView: {
    backgroundColor: Colors.modalBackground,
    padding: 12,
    borderRadius: 10,
    width: Screen.WIDTH - 100,
    alignItems: 'center',
  },
});


//open/ close modal
  const showModalLocationPermissions = () => {
    setModalLocationPermissionVisible(true);
  };

  const closeModalLocationPermissions = () => {
    setModalLocationPermissionVisible(false);
  };



//button open

 <WText onPress={showModalLocationPermissions} color={Colors.primary}>
              {translate('intro.moreInfo')}
            </WText>


//using
      <ModalPermissionLocation
        isVisible={modalLocationPermissionVisible}
        onClosedModal={closeModalLocationPermissions}
        text={translate('permission.wouldLikeToGetBackgroundLocation')}
      />
```

</details>



