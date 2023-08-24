package com.example.demo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import com.example.demo.entity.Employee;
import com.example.demo.repositories.EmployeeDAO;

import jakarta.servlet.http.HttpSession;

@Controller
class MyEmpController {
	@Autowired
	EmployeeDAO dao;
	
	@RequestMapping("")
	public String welcomePage()
	{
		return "index.jsp";
	}
	
	@RequestMapping("/create")
	@ResponseBody
	public String insertEmployeeRecord(Employee e)
	{
		dao.save(e);
		return "Successfully inserted Employee Record";
	}
	
	@RequestMapping("/retrieve")
	public String getAllEmployees(HttpSession session)
	{
		List<Employee> employees = dao.findAll();
		session.setAttribute("emps", employees);
		return "displayAll.jsp";
	}
	
	@RequestMapping("/search")
	@ResponseBody
	public String searchEmployee(int eid)
	{
		Employee e = dao.findById(eid).get();
		String str = null;
		str = "<br>Name : "+ e.getName();
		str = str + "<br>Age : "+ e.getAge();
		str = str + "<br>Salary : "+ e.getSalary();
		str = str + "<br>Designation : "+ e.getDesignation();
		return str;
	}
	
	@RequestMapping("/delete")
	@ResponseBody
	public String deleteEmployee(int eid)
	{
		dao.deleteById(eid);
		return "Successfully removed the employee record";
	}
	
	@RequestMapping("/update")
	@ResponseBody
	public String updateEmployee(Employee e)
	{
		dao.save(e);
		return "updated";
	}
}
