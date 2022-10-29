# Validation with formik
1. **yarn add formik** : install library
2. **yarn add yup** : install library
3. **Example use:**
```js
import { View, TextInput, StyleSheet } from 'react-native';
import { useFormik } from 'formik';
import * as Yup from 'yup';

const SignUpScreen = () => {

const formik = useFormik({
    initialValues: {
      firstName: '',
      lastName: '',
      email: '',
      password: '',
      confirmPassword: '',
      avatar: null,
    },
    validationSchema: Yup.object({
      firstName: Yup.string()
        .trim(translate('firstNameWhileSpace'))
        .strict(true)
        .matches(/^[a-zA-Z]+(([',. -’][a-zA-Z ])?[a-zA-Z]*)*$/g, translate('firstNameInvalid'))
        .min(2, translate('nameMin'))
        .max(70, translate('nameMax'))
        .required(translate('firstNameRequired')),

      lastName: Yup.string()
        .trim(translate('lastNameWhileSpace'))
        .strict(true)
        .matches(/^[a-zA-Z]+(([',. -’][a-zA-Z ])?[a-zA-Z]*)*$/g, translate('lastNameInvalid'))
        .min(2, translate('lastNameMin'))
        .max(70, translate('lastNameMax'))
        .required(translate('lastNameRequired')),
      email: Yup.string().email(translate('emailInvalid')).required(translate('emailRequired')),
      password: Yup.string()
        .min(6, translate('passwordMinMax'))
        .max(20, translate('passwordMinMax'))
        .required(translate('passwordRequired')),
      confirmPassword: Yup.string()
        .min(6, translate('confirmPasswordMinMax'))
        .max(20, translate('confirmPasswordMinMax'))
        .required(translate('confirmPasswordRequired'))
        .oneOf([Yup.ref('password'), null], translate('confirmPasswordNotMatch')),
    }),
    onSubmit: (values) => {
      onSignUp(values);
    },
  });

    return(
        <View>
            <TextInput
            />
        </View>
    )
}
export default LoginScreen;
const styles = StyleSheet.create({})
```