# apigee-cli
package com.pru.sonata;

import io.swagger.models.Info;
import io.swagger.models.Model;
import io.swagger.models.Operation;
import io.swagger.models.Path;
import io.swagger.models.Swagger;
import io.swagger.models.Tag;
import io.swagger.models.properties.Property;
import io.swagger.parser.SwaggerParser;

import java.util.List;
import java.util.Map;

public class RESTServicesParser {
	
	Swagger swagger;

	Info info;

	public static void main(String[] args) {
		RESTServicesParser rsp = new RESTServicesParser(
				"C:\\Users\\x1207359\\learning\\sonata-swagger.json");
		
		String directory = "C:\\Users\\x1207359\\learning\\sonata-swagger\\";
		try {
			rsp.process();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	private void process() throws Exception {		 
		List tagsList = swagger.getTags();
		for (Object tag : tagsList) {
			System.out.println(" The Object is " + ((Tag) tag).getName());
			//getPaths for the tag
			//getDefinitions for the paths
			//write them to a file.
			
		}
		Map<String, Path> pathDetails = swagger.getPaths();
		for (String key : pathDetails.keySet()) {
			Path path = pathDetails.get(key);
			List opList = path.getOperations();
			for (Object operation : opList) {
				System.out.println(" The Object is "
						+ ((Operation) operation).getTags());
			}
		}

		Map<String, Model> definitions = swagger.getDefinitions();
		for (String key : definitions.keySet()) {
			Model model = definitions.get(key);
			
			Map<String, Property> props = model.getProperties();
			for (String prop : props.keySet()) {
				Property property = props.get(prop);	
				String name = property.getType();
				String title = property.getDescription();
				System.out.println(" The Title is " + model.getTitle());
				System.out.println(" The Name is " + name);
			}
			System.exit(1);
		}
	}

	

	public RESTServicesParser(String path) {
		swagger = new SwaggerParser().read(path);
		info = swagger.getInfo();
	}

	public String getAPIBasePath() {
		return swagger.getBasePath();
	}

	public Map<String, Path> getAPIPaths() {
		return swagger.getPaths();
	}

	public Path getAPIPaths(String path) {
		return swagger.getPath(path);
	}

	public Map<String, Model> getAPIDefinitions() {
		return swagger.getDefinitions();
	}

	public String getAPITitle() {
		String title = info.getTitle();
		if (title == null)
			return null;
		title = title.replaceAll("\\s+", "");
		return title.toLowerCase();
	}

	public String getAPIVersion() {
		String version = info.getVersion();
		if (version == null)
			return null;
		final StringBuilder sb = new StringBuilder(version.length());
		for (int i = 0; i < version.length(); i++) {
			final char c = version.charAt(i);
			System.out.println(" char is " + c);
			if (c > 47 && c < 58) {
				sb.append(c);
			}
		}
		return sb.toString();
	}

}
