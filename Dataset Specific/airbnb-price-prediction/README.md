## Attributes

['id', 'log_price', 'property_type', 'room_type', 'amenities',
       'accommodates', 'bathrooms', 'bed_type', 'cancellation_policy',
       'cleaning_fee', 'city', 'description', 'first_review',
       'host_has_profile_pic', 'host_identity_verified', 'host_response_rate',
       'host_since', 'instant_bookable', 'last_review', 'latitude',
       'longitude', 'name', 'neighbourhood', 'number_of_reviews',
       'review_scores_rating', 'thumbnail_url', 'zipcode', 'bedrooms', 'beds']


## Basic Version - v1

Files Inclusive: 
```
AirBNB EDA_v1.1 - Imputation
AirBNB EDA_v1.2 - Binning, Dummy, Data Split

AirBnb Modelv1.1 - Linear Regression

Output (Processed Folder) 
		-	v1.1_data.csv
			v1.2_train.csv
			v1.2_test.csv
```

Algorithm Summary:
```
1. Drop Columns - ['id', 'description', 'first_review', 'host_response_rate', \
                      'host_since', 'last_review', 'latitude', 'longitude',\
                      'name', 'neighbourhood', 'thumbnail_url', 'zipcode']
                      
2. Imputation -
        log_price                     0
        property_type                 0
        room_type                     0
        accommodates                  0
        bathrooms                   200
        bed_type                      0
        cancellation_policy           0
        cleaning_fee                  0
        city                          0
        host_has_profile_pic        188
        host_identity_verified      188
        instant_bookable              0
        number_of_reviews             0
        review_scores_rating      16722
        bedrooms                     91
        beds                        131
        
    * impute_bedrooms_bathrooms_beds()
            Number of bedrooms = bathrooms and vice versa.
    * impute_hostHasPic_identityVerification()
        1. If verified: Profile pic must be present.
        2. If not verified: 
            if profile pic present: put the same in verified col.
    * impute_review_scores_rating()
            If No. of reviews = 0 then, Review Scores Rating = 0.
    
    Drop the rest of the rows containing missing values.
    
3. Type Conversion
4. Standardization - Not Needed here
5. Binning
6. Dummy Creation
    * This is only a transformation step which can be applied before splitting them.
    * property_type, room_type, bed_type, cancellation_policy, cleaning_fee, city, host_has_profile_pic, host_identity_verified, instant_bookable

Categorical
        property_type = 35 Types
        room_type = 3 Types
        bed_type = 5
        cancellation_policy = 5
        cleaning_fee = 2
        city = 6
        host_has_profile_pic = 2
        host_iden.. = 2
        instan.. = 2
        Total = 62. So, 16 + 62 - 9 = 69
        
More Preprocessing Required
    Amenitites - Textual Data (Needs to be categorised)
    
    After Dummy creation - 69 Columns
    
7. Test/Train Data Split
    Train Size - (51118, 69)
    Test Size - (21908, 69)

```
## Accuracy
1. Linear Regression Model
```
    Intercept:  4.488236417689669
    Training R2 Score:  0.568134210657812
    Test R2 Score:  0.563982991624809
    RMSE on Test:  0.4738668223683896
```