# Auto suggest Location with google API key

<details>
    <summary><b>AutocompletePlaceInput</b></summary>


```js



import React from 'react';
import { StyleSheet, View } from 'react-native';
import Config from 'react-native-config';
import {
  GooglePlaceData,
  GooglePlaceDetail,
  GooglePlacesAutocomplete,
  GooglePlacesAutocompleteRef,
} from 'react-native-google-places-autocomplete';

export type AutocompletePlaceInputProps = {
  title: string;
  inputRef: React.Ref<GooglePlacesAutocompleteRef> | undefined;
  onPress: ((data: GooglePlaceData, detail: GooglePlaceDetail | null) => void) | undefined;
  error: any;
};
export const AutocompletePlaceInput: React.FC<AutocompletePlaceInputProps> = ({
  title,
  inputRef,
  onPress,
  error,
}) => {
  return (
    <View>
      <View style={styles.autocompleteInputContainer}>
        <WText type="regular12" color={Colors.blackB30} mLeft={16} mTop={8}>
          {title || translate('account.address')}
        </WText>
        <GooglePlacesAutocomplete
          ref={inputRef}
          onPress={onPress}
          isRowScrollable={false}
          debounce={600}
          placeholder={translate('account.addressPlaceHolder')}
          query={{
            key: Config.GOOGLE_PLACE_API_KEY, //on env file
            language: 'en',
            components: 'country:ca',
          }}
          enablePoweredByContainer={false}
          numberOfLines={1}
          textInputProps={{}}
          styles={{
            textInput: styles.textInput,
          }}
        />
      </View>
      {error && (
        <WText type="regular12" color={Colors.primary} mTop={4}>
          {error}
        </WText>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  autocompleteInputContainer: {
    borderWidth: 1,
    borderRadius: 12,
    borderColor: Colors.blackB10,
    marginTop: 16,
    marginBottom: 1,
  },
  textInput: {
    paddingHorizontal: 16,
    fontFamily: 'Inter-Regular',
    fontSize: 14,
    maxHeight: 30,
  },
});

export default AutocompletePlaceInput;


```

<details>