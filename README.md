# CSC207-Project

Problem Domain: 
For our project, we will attempt to create a nutrition
application that will allow users to enter a desired daily caloric and
nutritional goal. The application will then provide sets of breakfast, lunch and
dinner recipes that will fit with their goals. Things such as a desired calorie
count, protein/carb/fat intake, and other nutritional goals will be taken into
account when choosing the recipes. 

Link to API:
https://developer.edamam.com/edamam-docs-recipe-api 

Some potential features:
- A way to save favourite recipes.
- Viewing each meal's nutritional information.
- Being able to set dietary restrictions.
- Saving preset settings for easy switching between calorie surplus/deficits.

An example call using hopscotch with the API is in the file adwadawd.png.

package api;
import entity.Team;
import okhttp3.*;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.IOException;
import java.sql.SQLOutput;
import java.util.Arrays;
import java.util.HashMap;

public class CallAPI {
    private static final String API_URL = "https://api.edamam.com/api/recipes/v2?type=public&app_id=981e8b83&" +
            "app_key=%202fba7f42e263a88f352970997e1158c3&calories=100-1000";

    public static String getApiUrl() {
        return API_URL;
    }

    public static void main(String[] args) {
        RecipeUrl();
    }

    public static JSONObject RecipeUrl() throws JSONException {
        OkHttpClient client = new OkHttpClient().newBuilder()
                .build();
        Request request = new Request.Builder()
                .url(String.format(CallAPI.getApiUrl()))
                .build();
        try {
            Response response = client.newCall(request).execute();
            System.out.println(response);
            JSONObject responseBody = new JSONObject(response.body().string());

            if (responseBody.getInt("status_code") == 200) {
                return responseBody.getJSONObject("hits");
            } else {
                throw new RuntimeException(responseBody.getString("message"));
            }
        } catch (IOException | JSONException e) {
            throw new RuntimeException(e);
        }
    }
}
