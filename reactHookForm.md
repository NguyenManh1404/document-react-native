# Validate form with react native hook form

1. yarn add react-hook-form

2. Custom **ControllerInput** component

```js
import React, {
  forwardRef,
  useCallback,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from 'react';
import {useController} from 'react-hook-form';
import {
  Keyboard,
  Platform,
  StyleSheet,
  TextInput,
  TouchableOpacity,
  View,
} from 'react-native';
import {LocalImage, Text} from '.';
import {APP_COLORS} from '../themes/colors';
import {FONT_SIZES, FONT_TYPES} from '../themes/fonts';
import {
  EMPTY_STRING,
  HIT_SLOP,
  INPUT_ICON_SIZE,
  IS_ANDROID,
} from '../utils/constants';

const Input = forwardRef(
  (
    {
      value,
      error,
      onBlur,
      style,
      multiline,
      inputContainerStyle,
      label,
      labelStyle,
      required,
      inputWrapperStyle,
      preIcon,
      placeholderTextColor,
      inputStyle,
      secureTextEntry,
      rightIcon,
      isClearButtonMode,
      innerRef,
      onSubmitEditing,
      nextField,
      name,
      touchedFields,
      ...props
    },
    ref,
  ) => {
    let inputRef = useRef();
    const [isSecureMode, setIsSecureMode] = useState(secureTextEntry);

    const borderColor = useMemo(
      () => (error ? APP_COLORS.error : APP_COLORS.neutralBlue),
      [error],
    );

    const toggleSecureState = useCallback(() => {
      setIsSecureMode(prevSecure => !prevSecure);
    }, []);

    const _onSubmitEditing = async e => {
      try {
        if (nextField) {
          const isValidated = await nextField?.trigger(name);
          if (isValidated && !touchedFields[nextField.name]) {
            nextField?.setFocus(nextField?.name);
            onSubmitEditing?.(e);
            return;
          }
        }
        if (!multiline) {
          Keyboard.dismiss();
        }
        onSubmitEditing?.(e);
      } catch (err) {}
    };

    useImperativeHandle(ref, () => ({
      focus: () => {
        inputRef.current?.focus();
      },
      blur: () => {
        inputRef.current?.blur();
      },
      clear: () => {
        inputRef.current?.clear();
      },
      setNativeProps: nativeProps => {
        inputRef.current?.setNativeProps(nativeProps);
      },
    }));

    return (
      <View style={style}>
        <View
          style={[
            styles.inputContainer,
            multiline ? styles.multiline : styles.notMultiline,
            inputContainerStyle,
            {borderColor},
          ]}>
          {label && (
            <Text
              type={value ? 'regular-12' : 'semiBold-12'}
              color={value ? APP_COLORS.neutral500Base : APP_COLORS.darkBlue}
              style={labelStyle}>
              {label}{' '}
              {required && (
                <Text
                  type={value ? 'regular-12' : 'semiBold-12'}
                  color={APP_COLORS.error}>
                  *
                </Text>
              )}
            </Text>
          )}

          <View
            style={[
              styles.inputWrapper,
              {
                marginTop: Platform.select({
                  ios: 5,
                  android: multiline ? 5 : 0,
                }),
              },
              inputWrapperStyle,
            ]}>
            {preIcon && (
              <LocalImage
                imageKey={preIcon?.imageKey}
                style={[styles.preIcon, preIcon?.style]}
              />
            )}

            <TextInput
              ref={e => {
                innerRef?.(e);
                inputRef = {
                  current: e,
                };
              }}
              placeholderTextColor={placeholderTextColor}
              style={[styles.input, inputStyle]}
              allowFontScaling={false}
              blurOnSubmit={false}
              secureTextEntry={isSecureMode}
              value={value}
              multiline={multiline}
              autoCapitalize={multiline ? 'sentences' : 'none'}
              textAlignVertical={multiline ? 'top' : 'auto'}
              onSubmitEditing={_onSubmitEditing}
              numberOfLines={multiline ? 5 : 1}
              {...props}
            />

            {secureTextEntry && (
              <TouchableOpacity onPress={toggleSecureState}>
                <LocalImage
                  imageKey={isSecureMode ? 'icEyeOpen' : 'icEyeClose'}
                  style={styles.secureIcon}
                  tintColor={APP_COLORS.inputIcon}
                />
              </TouchableOpacity>
            )}
            {rightIcon && (
              <TouchableOpacity
                disabled={!rightIcon?.onPress}
                onPress={rightIcon?.onPress}>
                <LocalImage
                  imageKey={rightIcon?.imageKey}
                  style={[styles.preIcon, rightIcon?.style]}
                />
              </TouchableOpacity>
            )}
            {isClearButtonMode && value?.length > 0 && (
              <TouchableOpacity
                onPress={() => {}}
                style={styles.btnClear}
                hitSlop={HIT_SLOP}>
                <LocalImage
                  imageKey={'icCloseCircle'}
                  style={styles.iconClearTextAndroid}
                />
              </TouchableOpacity>
            )}
          </View>
        </View>
        {error && (
          <Text
            type="regular-12"
            color={APP_COLORS.systemErrorDefault}
            style={styles.error}>
            {error?.message}
          </Text>
        )}
      </View>
    );
  },
);

const ControllerInput = ({
  secureTextEntry,
  style,
  inputStyle,
  labelStyle,
  required = false,
  label,
  preIcon,
  rightIcon,
  multiline,
  inputContainerStyle,
  inputWrapperStyle,
  placeholderTextColor = APP_COLORS.neutral500Base,
  isClearButtonMode,
  control,
  name,
  rules = {},
  nextField,
  onSubmitEditing,
  transformValue,
  onCustomChangeText,
  ...props
}) => {
  const {
    field: {value, onChange, onBlur, ref},
    fieldState: {error},
    formState: {touchedFields},
  } = useController({
    name,
    control,
    rules,
  });

  const onChangeText = text => {
    if (typeof transformValue === 'function') {
      const priceFormatRegex = /,/g;
      const transformedText = priceFormatRegex.test(text)
        ? transformValue(text.replace(/,/g, EMPTY_STRING))
        : transformValue(text);
      onChange(transformedText);
      onCustomChangeText?.(transformedText);
    } else {
      onChange(text);
      onCustomChangeText?.(text);
    }
  };

  return (
    <Input
      innerRef={ref}
      error={error}
      inputContainerStyle={inputContainerStyle}
      inputStyle={inputStyle}
      inputWrapperStyle={inputWrapperStyle}
      isClearButtonMode={isClearButtonMode}
      label={label}
      labelStyle={labelStyle}
      multiline={multiline}
      onBlur={onBlur}
      onChangeText={onChangeText}
      placeholderTextColor={placeholderTextColor}
      preIcon={preIcon}
      required={required}
      rightIcon={rightIcon}
      secureTextEntry={secureTextEntry}
      style={style}
      value={value}
      nextField={nextField}
      onSubmitEditing={onSubmitEditing}
      name={name}
      touchedFields={touchedFields}
      {...props}
    />
  );
};

const styles = StyleSheet.create({
  input: {
    flex: 1,
    fontSize: FONT_SIZES.medium,
    color: APP_COLORS.blackText,
    fontFamily: FONT_TYPES.semiBold,
    padding: 0,
  },
  error: {
    marginTop: 5,
  },
  preIcon: {
    width: 12,
    height: 12,
    tintColor: APP_COLORS.inputIcon,
    marginRight: 5,
  },
  secureIcon: {
    width: INPUT_ICON_SIZE,
    height: INPUT_ICON_SIZE,
    marginHorizontal: 5,
  },
  inputWrapper: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  multiline: {
    paddingBottom: IS_ANDROID ? 0 : 15,
    maxHeight: 150,
  },
  notMultiline: {
    maxHeight: 64,
  },
  inputContainer: {
    borderWidth: 1,
    borderRadius: 8,
    paddingLeft: 15,
    paddingRight: 8,
    paddingTop: 11,
    paddingBottom: 15,
  },
  btnClear: {
    width: 14,
    height: 14,
    borderRadius: 7,
    marginLeft: 5,
  },
  iconClearTextAndroid: {
    width: 16,
    height: 16,
  },
});

export default ControllerInput;

```

3. demo React hook form:

```js
import React from 'react';
import {useTranslation} from 'react-i18next';
import {StyleSheet, View} from 'react-native';

import {useForm} from 'react-hook-form';
import {
  Button,
  ControllerInput,
  KeyboardContainer,
  SafeAreaContainer,
  Text,
} from '../../components';
import SocialButton from '../../components/auth/SocialButton';
import {useAuthentication} from '../../hooks/auth/useAuthentication';
import {APP_COLORS} from '../../themes/colors';
import {EMAIL_REGEX, EMPTY_STRING} from '../../utils/constants';

const FORM_FIELDS = {
  EMAIL: 'email',
  PASSWORD: 'password',
};

const SignInByEmail = () => {
  const {t} = useTranslation();
  const {
    socialLoading,
    loading,
    loginByEmail,
    loginApple,
    loginFacebook,
    loginGoogle,
    isAppleSignInSupported,
    moveToForgotPassword,
  } = useAuthentication();

  const {control, handleSubmit, setFocus, trigger} = useForm({
    defaultValues: {
      email: __DEV__ ? 'vantao.dev+11@gmail.com' : EMPTY_STRING,
      password: __DEV__ ? '123456' : EMPTY_STRING,
    },
  });

  const onSubmit = values => {
    loginByEmail(values);
  };

  return (
    <SafeAreaContainer useBottomPadding loading={loading || socialLoading}>
      <KeyboardContainer style={styles.container} keyboardDismissMode="on-drag">
        <ControllerInput
          required
          keyboardType="email-address"
          style={styles.input}
          label={t('authentication.login.email')}
          placeholder={t('authentication.login.emailPlaceholder')}
          returnKeyType="next"
          control={control}
          name={FORM_FIELDS.EMAIL}
          rules={{
            required: t('validation.emailRequired'),
            pattern: {
              value: EMAIL_REGEX,
              message: t('validation.emailInvalid'),
            },
          }}
          nextField={{
            name: FORM_FIELDS.PASSWORD,
            setFocus,
            trigger,
          }}
        />

        <ControllerInput
          required
          style={styles.input}
          label={t('authentication.login.password')}
          placeholder={t('authentication.login.passwordPlaceholder')}
          secureTextEntry
          control={control}
          name={FORM_FIELDS.PASSWORD}
          rules={{
            required: t('validation.passwordRequired'),
            maxLength: {
              value: 20,
              message: t('validation.passwordMinMax'),
            },
            minLength: {
              value: 6,
              message: t('validation.passwordMinMax'),
            },
          }}
        />
        <View style={styles.guestLoginContainer}>
          <Text
            style={styles.touchableText}
            color={APP_COLORS.blackText}
            onPress={moveToForgotPassword}>
            {t('authentication.login.forgotPassword')}
          </Text>
        </View>

        <Button
          label={t('button.login')}
          style={styles.button}
          onPress={handleSubmit(onSubmit)}
        />

        <Text
          color={APP_COLORS.blackText}
          textAlign="center"
          style={styles.loginWith}>
          {t('authentication.login.orLoginWith')}
        </Text>

        <View style={styles.socialButtonContainer}>
          <SocialButton imageKey="logoFacebook" onPress={loginFacebook} />
          {isAppleSignInSupported && (
            <SocialButton imageKey="appleLogo" onPress={loginApple} />
          )}
          <SocialButton imageKey="googleLogo" onPress={loginGoogle} />
        </View>
      </KeyboardContainer>
    </SafeAreaContainer>
  );
};

const styles = StyleSheet.create({
  container: {
    paddingHorizontal: 24,
  },
  guestLoginContainer: {
    flexDirection: 'row',
    justifyContent: 'flex-end',
  },
  touchableText: {
    marginRight: 5,
  },
  button: {
    marginVertical: 30,
  },

  input: {
    marginBottom: 15,
  },
  loginWith: {
    paddingVertical: 18,
  },
  socialButtonContainer: {
    flexDirection: 'row',
    justifyContent: 'space-evenly',
    marginBottom: 48,
    marginTop: 10,
  },
});

export default SignInByEmail;

```